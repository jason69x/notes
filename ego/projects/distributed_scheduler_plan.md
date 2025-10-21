
## Simplified Architecture

```
Client ─(gRPC)→ Leader ─(gRPC)→ Workers (worker1, worker2, worker3)
                  ↑                 │
                  └─(heartbeat)─────┘
```

**Key Simplification:** 
- Client sends request → waits → gets response (synchronous)
- No task queue needed! Just forward immediately
- No status checking needed

### Afternoon (4-6 hours): Project Setup

**1. Project Structure (Simple Version)**
```
distributed-scheduler/
├── proto/
│   └── scheduler.proto       # All gRPC definitions in ONE file
├── cmd/
│   ├── leader/main.go        # Leader executable
│   ├── worker/main.go        # Worker executable
│   └── client/main.go        # Client executable
├── Dockerfile.leader
├── Dockerfile.worker
├── docker-compose.yml
├── go.mod
└── go.sum
```

**2. Create scheduler.proto (Simplified)**
```protobuf
syntax = "proto3";
package scheduler;
option go_package = "./proto";

// Client → Leader
service Scheduler {
  rpc CheckPrime(PrimeRequest) returns (PrimeResponse);
}

message PrimeRequest {
  int64 number = 1;
}

message PrimeResponse {
  bool is_prime = 1;
  string processed_by = 2;  // which worker did it
}

// Leader → Worker
service Worker {
  rpc ProcessPrime(PrimeRequest) returns (PrimeResponse);
  rpc Heartbeat(HeartbeatRequest) returns (HeartbeatResponse);
}

message HeartbeatRequest {
  string worker_id = 1;
}

message HeartbeatResponse {
  bool alive = 1;
}
```

**3. Initialize Project**
```bash
mkdir distributed-scheduler
cd distributed-scheduler

# Initialize Go module
go mod init distributed-scheduler

# Create directories
mkdir -p proto cmd/leader cmd/worker cmd/client

# Copy proto file above into proto/scheduler.proto

# Generate Go code
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest

protoc --go_out=. --go-grpc_out=. proto/scheduler.proto
```

---

## Day 2: Implement Basic Client-Leader-Worker (8-10 hours)

### Morning (4 hours): Worker Implementation

**cmd/worker/main.go** (Simplified - ~80 lines)
```go
package main

import (
    "context"
    "fmt"
    "log"
    "net"
    "os"
    "time"
    
    pb "distributed-scheduler/proto"
    "google.golang.org/grpc"
)

type WorkerServer struct {
    pb.UnimplementedWorkerServer
    workerID string
}

func (s *WorkerServer) ProcessPrime(ctx context.Context, req *pb.PrimeRequest) (*pb.PrimeResponse, error) {
    log.Printf("Processing prime check for: %d", req.Number)
    
    isPrime := checkPrime(req.Number)
    
    return &pb.PrimeResponse{
        IsPrime:     isPrime,
        ProcessedBy: s.workerID,
    }, nil
}

func (s *WorkerServer) Heartbeat(ctx context.Context, req *pb.HeartbeatRequest) (*pb.HeartbeatResponse, error) {
    return &pb.HeartbeatResponse{Alive: true}, nil
}

func checkPrime(n int64) bool {
    if n <= 1 {
        return false
    }
    for i := int64(2); i*i <= n; i++ {
        if n%i == 0 {
            return false
        }
    }
    return true
}

func main() {
    workerID := os.Getenv("WORKER_ID")
    port := os.Getenv("PORT")
    
    lis, err := net.Listen("tcp", ":"+port)
    if err != nil {
        log.Fatalf("Failed to listen: %v", err)
    }
    
    s := grpc.NewServer()
    pb.RegisterWorkerServer(s, &WorkerServer{workerID: workerID})
    
    log.Printf("Worker %s listening on port %s", workerID, port)
    
    if err := s.Serve(lis); err != nil {
        log.Fatalf("Failed to serve: %v", err)
    }
}
```

### Afternoon (4-6 hours): Leader Implementation

**cmd/leader/main.go** (Simplified - ~120 lines)
```go
package main

import (
    "context"
    "fmt"
    "log"
    "net"
    "sync"
    "time"
    
    pb "distributed-scheduler/proto"
    "google.golang.org/grpc"
)

type LeaderServer struct {
    pb.UnimplementedSchedulerServer
    workers      []string          // List of worker addresses
    workerIndex  int               // For round-robin
    workerMutex  sync.Mutex
}

func (s *LeaderServer) CheckPrime(ctx context.Context, req *pb.PrimeRequest) (*pb.PrimeResponse, error) {
    // Select next worker (round-robin)
    s.workerMutex.Lock()
    if len(s.workers) == 0 {
        s.workerMutex.Unlock()
        return nil, fmt.Errorf("no workers available")
    }
    workerAddr := s.workers[s.workerIndex]
    s.workerIndex = (s.workerIndex + 1) % len(s.workers)
    s.workerMutex.Unlock()
    
    log.Printf("Forwarding request to %s", workerAddr)
    
    // Connect to worker and forward request
    conn, err := grpc.Dial(workerAddr, grpc.WithInsecure())
    if err != nil {
        return nil, err
    }
    defer conn.Close()
    
    client := pb.NewWorkerClient(conn)
    return client.ProcessPrime(ctx, req)
}

func (s *LeaderServer) monitorWorkers() {
    ticker := time.NewTicker(5 * time.Second)
    for range ticker.C {
        s.workerMutex.Lock()
        aliveWorkers := []string{}
        
        for _, workerAddr := range s.workers {
            if s.isWorkerAlive(workerAddr) {
                aliveWorkers = append(aliveWorkers, workerAddr)
                log.Printf("Worker %s is alive", workerAddr)
            } else {
                log.Printf("Worker %s is DEAD", workerAddr)
            }
        }
        
        s.workers = aliveWorkers
        s.workerMutex.Unlock()
    }
}

func (s *LeaderServer) isWorkerAlive(workerAddr string) bool {
    conn, err := grpc.Dial(workerAddr, grpc.WithInsecure())
    if err != nil {
        return false
    }
    defer conn.Close()
    
    client := pb.NewWorkerClient(conn)
    ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
    defer cancel()
    
    _, err = client.Heartbeat(ctx, &pb.HeartbeatRequest{})
    return err == nil
}

func main() {
    // Hardcode worker addresses (Docker will resolve these)
    workers := []string{
        "worker1:50052",
        "worker2:50053",
        "worker3:50054",
    }
    
    leader := &LeaderServer{
        workers: workers,
    }
    
    // Start heartbeat monitoring
    go leader.monitorWorkers()
    
    lis, err := net.Listen("tcp", ":50051")
    if err != nil {
        log.Fatalf("Failed to listen: %v", err)
    }
    
    s := grpc.NewServer()
    pb.RegisterSchedulerServer(s, leader)
    
    log.Println("Leader listening on port 50051")
    
    if err := s.Serve(lis); err != nil {
        log.Fatalf("Failed to serve: %v", err)
    }
}
```

---

## Day 3: Docker + Client + Testing (8-10 hours)

### Morning (4 hours): Docker Setup

**Dockerfile.leader**
```dockerfile
FROM golang:1.21-alpine
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN go build -o leader ./cmd/leader
CMD ["./leader"]
```

**Dockerfile.worker**
```dockerfile
FROM golang:1.21-alpine
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN go build -o worker ./cmd/worker
CMD ["./worker"]
```

**docker-compose.yml** (Simplified)
```yaml
version: '3.8'

services:
  leader:
    build:
      context: .
      dockerfile: Dockerfile.leader
    ports:
      - "50051:50051"
    networks:
      - scheduler-net

  worker1:
    build:
      context: .
      dockerfile: Dockerfile.worker
    environment:
      - WORKER_ID=worker1
      - PORT=50052
    networks:
      - scheduler-net
    depends_on:
      - leader

  worker2:
    build:
      context: .
      dockerfile: Dockerfile.worker
    environment:
      - WORKER_ID=worker2
      - PORT=50053
    networks:
      - scheduler-net
    depends_on:
      - leader

  worker3:
    build:
      context: .
      dockerfile: Dockerfile.worker
    environment:
      - WORKER_ID=worker3
      - PORT=50054
    networks:
      - scheduler-net
    depends_on:
      - leader

networks:
  scheduler-net:
    driver: bridge
```

### Afternoon (4-6 hours): Client + Testing

**cmd/client/main.go** (Simple CLI)
```go
package main

import (
    "context"
    "fmt"
    "log"
    "time"
    
    pb "distributed-scheduler/proto"
    "google.golang.org/grpc"
)

func main() {
    conn, err := grpc.Dial("leader:50051", grpc.WithInsecure())
    if err != nil {
        log.Fatalf("Connection failed: %v", err)
    }
    defer conn.Close()
    
    client := pb.NewSchedulerClient(conn)
    
    // Test with multiple numbers
    numbers := []int64{17, 18, 19, 20, 21, 22, 23, 24, 25}
    
    for _, num := range numbers {
        ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
        
        resp, err := client.CheckPrime(ctx, &pb.PrimeRequest{Number: num})
        if err != nil {
            log.Printf("Error checking %d: %v", num, err)
        } else {
            fmt.Printf("%d is prime: %v (processed by %s)\n", 
                num, resp.IsPrime, resp.ProcessedBy)
        }
        
        cancel()
    }
}
```

**Test it:**
```bash
# Build and start
docker-compose up --build

# In another terminal, run client
docker-compose run client go run cmd/client/main.go

# Expected output:
# 17 is prime: true (processed by worker1)
# 18 is prime: false (processed by worker2)
# 19 is prime: true (processed by worker3)
# ...
```

**Test failure:**
```bash
# Kill a worker
docker-compose stop worker2

# Run client again - should still work with 2 workers
# Check leader logs - should show "Worker worker2 is DEAD"
```

---

## Day 4: Bully Algorithm (8-10 hours)

### Simplified Bully Algorithm

**Key Idea:**
- Each node has an ID (worker1=1, worker2=2, worker3=3, leader=4)
- When leader dies, nodes with higher IDs compete
- Highest ID wins and announces itself

**Add to proto/scheduler.proto:**
```protobuf
// Add to Worker service
rpc Election(ElectionRequest) returns (ElectionResponse);
rpc Victory(VictoryRequest) returns (VictoryResponse);

message ElectionRequest {
  int32 candidate_id = 1;
}

message ElectionResponse {
  bool ok = 1;
}

message VictoryRequest {
  int32 new_leader_id = 1;
  string new_leader_addr = 2;
}

message VictoryResponse {
  bool acknowledged = 1;
}
```

**Implementation Steps:**

1. **Each worker stores:**
   - Its own ID
   - List of all peer addresses (including leader)
   - Current leader address

2. **When heartbeat to leader fails 3 times:**
   - Start election
   - Send election message to all nodes with higher ID
   - If no response in 2 seconds, declare victory
   - Announce victory to all nodes

3. **Workers become the new leader:**
   - Start accepting client requests
   - Other workers forward to new leader

**Simplified Implementation (~150 lines added to worker/main.go)**

This is the most complex part. Focus on:
- Detecting leader failure (3 missed heartbeats)
- Sending election messages to higher-ID nodes
- Timeout logic (if no response, you win)
- Victory announcement
- 
## Quick Commands Reference

```bash
# Generate protobuf
protoc --go_out=. --go-grpc_out=. proto/scheduler.proto

# Build and run
docker-compose up --build

# View logs
docker-compose logs -f leader
docker-compose logs -f worker1

# Stop everything
docker-compose down

# Run client
docker-compose exec leader go run /app/cmd/client/main.go

# Test manually with grpcurl (optional)
grpcurl -plaintext -d '{"number": 17}' localhost:50051 scheduler.Scheduler/CheckPrime
```
