# Lab 2: gNB TDD Traffic Analysis

## 1. Goal

In this lab, you will compare three 5-second UDP iperf3 tests. You will learn how TDD slot allocation can affect downlink and uplink performance.

You do not need to understand every packet. Follow the steps, complete the tables, and answer the short questions.

## 2. Test Scenarios

| Scenario | TDD setting | Traffic direction | Main idea |
|----------|-------------|-------------------|-----------|
| A | DL-heavy | Downlink | The TDD setting favors the traffic direction |
| B | DL-heavy | Uplink | There are fewer opportunities for uplink traffic |
| C | Balanced | Uplink | More uplink opportunities are available than in B |

| Scenario | iperf3 JSON | Wireshark capture |
|----------|-------------|-------------------|
| A | `scenario-a-downlink.json` | `scenario-a-dl-heavy-downlink.pcapng` |
| B | `scenario-b-uplink.json` | `scenario-b-dl-heavy-uplink.pcapng` |
| C | `scenario-c-uplink.json` | `scenario-c-balanced-uplink.pcapng` |

## 3. Task 1 — Confirm the Traffic Direction

- `reverse: 1` means downlink in this lab.
- `reverse: 0` means uplink in this lab.

After opening each JSON file, I found the value at:

```text
start → test_start → reverse
```

This is the data I obtained, shown in the completed table below:

| Scenario | Reverse value | Direction |
|----------|---------------|-----------|
| A | 1 | downlink |
| B | 0 | uplink |
| C | 0 | uplink |

Questions:

1. Does the traffic direction shown in the JSON match the scenario description? Show the evidence.

Yes, the traffic direction shown in the JSON matches the descriptions from the scenarios. See Evidence 1-3

![JSON scenario A](/readMeImages/lab2/image1.png)
*Evidence 1 — start → test_start → reverse from scenario A*

![JSON scenario B](/readMeImages/lab2/image2.png)
*Evidence 2 — start → test_start → reverse from scenario B*

![JSON scenario C](/readMeImages/lab2/image3.png)
*Evidence 3 — start → test_start → reverse from scenario C*
## 4. Task 2 — Compare Throughput

In each JSON file, I found the bits per second at:

```text
end → sum → bits_per_second
```

![JSON scenario A](/readMeImages/lab2/image4.png)
*Evidence 4 — end → sum → bits_per_second from scenario A*

![JSON scenario B](/readMeImages/lab2/image5.png)
*Evidence 5 — end → sum → bits_per_second from scenario B*

![JSON scenario C](/readMeImages/lab2/image6.png)
*Evidence 6 — end → sum → bits_per_second from scenario C*

I converted the value to Mbps with this formula:

```text
Throughput (Mbps) = bits_per_second ÷ 1,000,000
```

This is the data I obtained, shown in the completed table below:

| Scenario | Direction | Throughput (Mbps) |
|----------|-----------|-------------------|
| A | Downlink | 90,6541 |
| B | Uplink | 109,1172 |
| C | Uplink | 124,4171 |

I calculated how much C improves over B with this formula:

```text
Improvement (%) = (C - B) ÷ B × 100
```

C improves over B by 14,02 %

Questions:

1. What is the downlink throughput in Scenario A?

    The downlink throughput in scenario A is 90,6541 Mbps.

2. What is the uplink throughput in Scenario B?

    The uplink throughput in scenario B is 109,1172 Mbps.

3. Is the uplink throughput higher in B or C?

    The uplink throughput is higher in scenario C.

4. By approximately what percentage does C improve over B?

    Scenario C has an uplink throughput approximately 14,02% better than Scenario B.

## 5. Task 3 — Check the Trade-off

In each JSON file, I found the lost percent and the jitter (in ms) at:

```text
end → sum → lost_percent
end → sum → jitter_ms
```
![JSON scenario B](/readMeImages/lab2/image7.png)
*Evidence 7 — end → sum → lost_percent/jitter_ms from scenario B*

![JSON scenario C](/readMeImages/lab2/image8.png)
*Evidence 8 — end → sum → lost_percent/jitter_ms from scenario C*

This is the data I obtained, shown in the completed table below:

| Scenario | Throughput (Mbps) | Packet loss (%) | Jitter (ms) |
|----------|-------------------|-----------------|-------------|
| B | 109,1172 | 0,2356 | 0,1161 |
| C | 124,4171 | 2,1339 | 0,1620 |

Questions:

1. Does C have higher uplink throughput than B?

    Yes, Scenario C has a higher uplink throughput than Scenario B (124,4171 Mbps vs 109,1172 Mbps).

2. Does C have lower packet loss than B?

    No, Scenario C has a higher packet loss than Scenario B (2,1339% vs 0,2356%).

3. What is the trade-off when changing from B to C?

    The trade-off is that changing from B to C increases your overall uplink throughput, but significantly increasing packet loss and slightly increasing jitter.

Scenario C has higher throughput, but higher packet loss than Scenario B. Therefore, the balanced TDD setting improves throughput, but it may reduce stability (higher packet loss % and jitter).

## 6. Task 4 — Is the TDD Setting Suitable?

Completed table, based on the scenario descriptions and my throughput results:

| Scenario | TDD setting | Traffic direction | Do they match? |
|----------|-------------|-------------------|----------------|
| A | DL-heavy | Downlink | |
| B | DL-heavy | Uplink | |
| C | Balanced | Uplink | |

Question:

1. Why might Scenario C achieve better uplink throughput than Scenario B?

## 7. Task 5 — Find GTP-U Information

After opening a `.pcapng` file in Wireshark, I entered this display filter:

```text
gtp && ip
```

I chose a packet that contains two IPv4 headers. In the packet details, I expanded:

1. the first `Internet Protocol Version 4` header;
2. `User Datagram Protocol`;
3. `GPRS Tunneling Protocol`; and
4. the second `Internet Protocol Version 4` header.

I recorded the following information from one packet:

| Item | Value |
|------|-------|
| Packet number | |
| Outer source IP | |
| Outer destination IP | |
| Inner source IP | |
| Inner destination IP | |
| TEID | |

## 8. Task 6 — TCP Retransmissions

The performance traffic is UDP, but iperf3 also uses a small TCP control connection.

After applying this Wireshark filter:

```text
tcp.analysis.retransmission
```

I recorded the number of displayed packets:

| Scenario | Displayed TCP retransmissions |
|----------|-------------------------------|
| A | |
| B | |
| C | |

Question:

1. Do these TCP retransmissions directly represent lost UDP performance packets?

## 9. Task 7 — RTT and Packet Interval

### TCP RTT

I applied this filter:

```text
tcp.analysis.ack_rtt
```

I selected one packet and recorded its ACK RTT:

| Scenario | Example TCP ACK RTT |
|----------|---------------------|
| A, B, or C | |

This value is the RTT of the TCP control connection. It is not the RTT of the UDP data packets.

### Packet interval

I selected two nearby packets from the same flow. In the packet details, I found:

```text
Time delta from previous displayed frame
```

One example:

| Flow | Packet numbers | Time interval |
|------|----------------|---------------|
| | | |

Packet interval means the time between two packets seen at the capture point. It is not the same as RTT.

## 10. Final Questions

1. Does Scenario A use a TDD setting that matches its traffic direction?
2. Why is Scenario B expected to have limited uplink resources?
3. How much does uplink throughput change from B to C?
4. What negative result is visible in C?
5. Why can increasing UL slots improve uplink throughput?
6. Is adding more UL slots always the best solution? Explain using throughput and packet loss.
7. Suggest one simple follow-up test that could improve throughput without producing too much packet loss.