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

