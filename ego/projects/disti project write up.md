##### CS-542 Project Writeup : Building a Minimal Distributed Task Scheduler in Go

###### Topic Explanation -

A distributed task scheduler is a system that manages and executes tasks across multiple networked nodes, enabling efficient resource utilization, fault tolerance, and scalability in computing environments.  This project focuses on building a minimal version in Go, leveraging the language's strong concurrency primitives like goroutines and channels for handling parallel task execution and communication. The scheduler will accept tasks (e.g., simple computations or scripts), queue them, and distribute them to worker nodes via HTTP or gRPC, with basic scheduling logic for timed or recurring jobs. Motivation stems from real-world applications in cloud computing and microservices, where such systems power tools like Kubernetes or Apache Airflow. Given my basic Go knowledge (variables, loops, basic concurrency), this project will serve as a hands-on learning opportunity to deepen expertise in distributed systems while creating a proof-of-concept prototype.

###### List of Relevant Materials -

- Andrew S. Tanenbaum, *Distributed Systems*.
- Kleppmann, M. *Designing Data-Intensive Applications*.
- Bodner, J. *Learning Go*.

Name - Abhishek Meena
Roll No - 254101002
Course - M.tech CSE (1st yr)
