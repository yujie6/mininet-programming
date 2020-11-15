# Mininet Answers
Before I start, I use 
```
sh ovs-ofctl add-flow s1 action=normal
```
to enable auto update of flow table.

## Q1. Link Latency and Throughput

First we check latency of the five links by 
```
$ s1 ping -c 20 s2 > latency_L1.txt
$ s2 ping -c 20 s3 > latency_L2.txt
$ s3 ping -c 20 s4 > latency_L3.txt
$ s2 ping -c 20 s5 > latency_L4.txt
$ s3 ping -c 20 s6 > latency_L5.txt
```
Similarly we can measure the throughput by
```bash
$ h1 ./iperfer -s -p 2048  
$ h2 ./iperfer -c -h 10.0.0.1 -p 2048 -t 20  
```

## Q2. Path Latency and Throughput
Similarly
```
$ h1 ping -c 20 h4 > latency_Q2.txt
$ h1 ./iperfer -s -p 2048
$ h4 ./iperfer -c -h 10.0.0.1 -p 2048 -t 20 
```

## Q3. Effects of Multiplexing
I use 
```
h1 ping -c 20 h4 
h8 ping -c 20 h10
```
simultaneously. And get
```
rtt min/avg/max/mdev = 160.482/161.169/164.523/0.854 ms
rtt min/avg/max/mdev = 160.298/161.088/163.099/0.564 ms
```
respectively.

## Q4: Effects of Latency
First we use 
