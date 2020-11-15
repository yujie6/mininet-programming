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
Latency should be the sum of latency of L1,L2,L3. While throughput should be the
minimum throughput of L1,L2,L3.

Similarly
```
$ h1 ping -c 20 h4 > latency_Q2.txt
$ h1 ./iperfer -s -p 2048
$ h4 ./iperfer -c -h 10.0.0.1 -p 2048 -t 20 
```
The result is
```
rtt min/avg/max/mdev = 160.391/161.177/164.771/0.905 ms
received=57320 KB rate=18.648 Mbps
```
Which matches our prediction.

## Q3. Effects of Multiplexing
I predict that the latency would not change, while the throughput will 
decrease such that the sum of each pair's share would not exceed the max
latency of the link.
 
I use 
```
h1 ping -c 20 10.0.0.4 
h8 ping -c 20 10.0.0.10
h7 ping -c 20 10.0.0.9
```
simultaneously. And get
```
rtt min/avg/max/mdev = 160.482/161.169/164.523/0.854 ms
rtt min/avg/max/mdev = 160.298/161.088/163.099/0.564 ms
rtt min/avg/max/mdev = 160.355/161.023/164.223/0.789 ms
```
respectively.
Which matches our prediction

Similarly we use iperfer on the 3 pairs. And the result matches our prediction as well.


## Q4: Effects of Latency
Denote `T(L)` be the throughput of L.

1. My prediction for latency: the latency should be the same when there is only one pair communicating with each other.
2. My prediction for throughput: the throughput of h1 and h4 should be `min(T(L1),T(L2)/2,T(L3))`.
Similarly the throughput of h5 and h6 should be `min(T(L4),T(L2)/2,T(L5))`
3. Actual latency for h1 and h4
`rtt min/avg/max/mdev = 160.338/160.884/163.453/0.632 ms` is almost the same as the one in Q3. 
And the same for h5 to h6.
4. Actual throughput for h1 and h4: `18.505` is very close to our prediction `min(18.938, 38/2, 28)`.
And for h5-h6, the throughput is `23.748`, slightly bigger than `min(23,38/2,23)`. 
This is because the share of throughput of L2 may not be equal. Since we still can verify 
that the sum of the two actual throughput
is almost `T(L2)` 
