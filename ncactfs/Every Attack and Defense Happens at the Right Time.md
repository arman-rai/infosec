📁 Category: Forensics

🧠 Description:
Someone tried to breach a system—but both the attack and the defense were caught... in motion. You’ve been given a PCAP file that recorded the traffic during the incident. The trick? Not everything is obvious at first glance. Look closely at the timestamps, sequence of packets, and timing of requests and responses.

The clue isn’t just what was sent… but when. Analyze the traffic. Reconstruct the event. Find the flag.

`Created By: ashwin7077`

# Documentation
At first I viewed the capture properties file 

|                |                                                                  |
| -------------- | ---------------------------------------------------------------- |
| Name:          | /home/namura/ncactfs/nca.pcapng                                  |
| Length:        | 18 kB                                                            |
| Hash (SHA256): | 245244f420552c2a93ed366963f3309c5ac598e4c572c12cc2639cf3deb34dc7 |
| Hash (SHA1):   | edd9555ce7a569e1d7bea76ce89b88b33fff7fbb                         |
| Format:        | Wireshark/... - pcapng                                           |
| Encapsulation: | Linux cooked-mode capture v1                                     |

Time

|               |                     |
| ------------- | ------------------- |
| First packet: | 2025-06-19 21:53:07 |
| Last packet:  | 2025-06-19 22:13:30 |
| Elapsed:      | 00:20:23            |

Capture

|              |                                                              |
| ------------ | ------------------------------------------------------------ |
| Hardware:    | 11th Gen Intel(R) Core(TM) i5-1135G7 @ 2.40GHz (with SSE4.2) |
| OS:          | Linux 6.12.25-amd64                                          |
| Application: | Dumpcap (Wireshark) 4.4.6                                    |

Interfaces

|           |                 |                |                              |                             |
| --------- | --------------- | -------------- | ---------------------------- | --------------------------- |
| Interface | Dropped packets | Capture filter | Link type                    | Packet size limit (snaplen) |
| any       | 0 (0.0%)        | none           | Linux cooked-mode capture v1 | 262144 bytes                |

Statistics

|   |   |   |   |
|---|---|---|---|
|Measurement|Captured|Displayed|Marked|
|Packets|92|92 (100.0%)|—|
|Time span, s|1223.361|1223.361|—|
|Average pps|0.1|0.1|—|
|Average packet size, B|165|165|—|
|Bytes|15212|15212 (100.0%)|0|
|Average bytes/s|12|12|—|
|Average bits/s|99|99|—|