*a distributed system is a collection of independent computers that appears to its users as a single coherent system*

###### Synchronization -

modern computers use laser cutted quartz crystals that resonate at particular frequency to calculate time and synchronize with NTP/GPS servers to get more precision bcoz quartz time may drift. computers have two kind of time : 
1. wall-clock time
		comes from RTC - real time clock chip (quartz crystal inside) + corrections from NTP/GPS
		can jump forward or backward when corrected
2. monotonic time
		a strictly increasing time used for measuring durations
		based on CPU times like TSC - time stamp counter 64 bit, HPET
		never goes backward no matter what happens to wall-clock time
		counts ticks since boot

RTC chip stores current time and date , os reads on boot

leap seconds, on 30 june and 31 december at 23:59:59 UTC , clock will either add/subtract 1 second or do nothing.
unix time
smearing,strata

![[ntp.png]]


slew - converge , step - jump

solar day, interval between two consecutive transit (event of sun's reaching its highest apparent point in the sky)

earth rotation is slowing down. 300 million years ago earth took ~400 days to revolve around the sun.

TAI time is 3ms behind solar time. if it remains unsynced we will have afternoon in the morning time. that why leap seconds were introduced. UTC = TAI + leap seconds

stratum levels

berkeley unix algorithm, time daemon ask polls every machine from time to time to ask what time it is there. based on the answers it calculates average of their time and tell all the other machines to advance or slow their clocks to the new time. suitable for system with no UTC receiver. time daemons time must be set manually by the operator periodically. internal clock synchronization algorithm.

---
##### Chp.3 Processes -

scalable system - increasing amount of work done by adding more resources.
vertical scalability, horizontal scalability

interrupts may  cause a considerable, and relatively long-lasting reorganization of the cache,in turn, affecting the overall performance of an application.

user-level threads(go routines)->mapped by go runtime to -> os threads (kernel threads) -> mapped by os kernel to -> physical threads (8c/16t).

go runtime creates kernel threads by using system calls and then schedules go routines on these threads. scheduling,synchronization,context switching between go routines is handled in user space without sys calls.

*TLP - thread level parallelism* value

Threads make it possible to retain the idea of sequential processes that make blocking system calls and still achieve parallelism.

*types of virtual machines :* 
1.  process virtual machine - runtime system (provides abstract instruction set to execute applications eg. jrt)
2.  native virtual machine monitor - installed on top of hardware (need to implement device drivers)
3.  hosted virtual machine monitor - installed on top of existing operating system

virtual machines offers a means to run applications relying on a specific operating environment.

a *container* can be thought of a collection of binaries (also called images) that jointly constitute the software environment for running applications.

1. *namespaces* , by which a collection of processes associated with a container is given their own view of identifiers and such, independent of other containers.
2. *Union file system* , which allows to combine several file systems into a layered fashion with only the highest layer allowing for write operations.
3. *control groups* - cgroups, by which resource restriction can be imposed upon a collection of processes.

base image, specific images

*kubernetes* is an open source system for automating deployment, scaling, and management of containerized applications.

*amazon elastic compute cloud - ec2* , lets you rent virtual servers (instances) in the cloud.

*amazon simple storage service - s3*

*thin-client approach* - everything is processed and stored at the server.

every browser tab gets its own renderer process.


