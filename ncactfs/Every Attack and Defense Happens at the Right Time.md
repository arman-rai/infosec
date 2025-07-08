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

And the protocol stats too
"Protocol","Percent Packets","Packets","Percent Bytes","Bytes","Bits/s","End Packets","End Bytes","End Bits/s","PDUs"
"Frame",100,92,100,15212,99.47679886706769,0,0,0,92
"Linux cooked-mode capture",100,92,12.463844333420983,1896,12.398633358661604,0,0,0,92
"Internet Protocol Version 6",9.782608695652174,9,2.682093084407047,408,2.66806034300313,0,0,0,9
"Internet Control Message Protocol v6",9.782608695652174,9,3.47094399158559,528,3.4527839732981684,9,528,3.4527839732981684,9
"Internet Protocol Version 4",46.73913043478261,43,5.653431501446227,860,5.623852683781108,0,0,0,43
"User Datagram Protocol",8.695652173913043,8,0.4207204838285564,64,0.4185192694906871,0,0,0,8
"Dynamic Host Configuration Protocol",8.695652173913043,8,21.930055219563503,3336,21.81531692220206,8,3336,21.81531692220206,8
"Internet Control Message Protocol",38.04347826086956,35,46.01630291874836,7000,45.775545100543894,35,7000,45.775545100543894,35
"Address Resolution Protocol",43.47826086956522,40,7.362608466999737,1120,7.324087216087023,40,1120,7.324087216087023,40

Some endpoints were:
[
    {
        "AS Number": "",
        "AS Organization": "",
        "Address": "8.8.8.8",
        "Bytes": "8260",
        "City": "",
        "Country": "",
        "Latitude": "",
        "Longitude": "",
        "Packets": "35",
        "Rx Bytes": "4248",
        "Rx Packets": "18",
        "Tx Bytes": "4012",
        "Tx Packets": "17"
    },
    {
        "AS Number": "",
        "AS Organization": "",
        "Address": "10.0.2.15",
        "Bytes": "8260",
        "City": "",
        "Country": "",
        "Latitude": "",
        "Longitude": "",
        "Packets": "35",
        "Rx Bytes": "4012",
        "Rx Packets": "17",
        "Tx Bytes": "4248",
        "Tx Packets": "18"
    },
    {
        "AS Number": "",
        "AS Organization": "",
        "Address": "192.168.41.2",
        "Bytes": "3688",
        "City": "",
        "Country": "",
        "Latitude": "",
        "Longitude": "",
        "Packets": "8",
        "Rx Bytes": "1320",
        "Rx Packets": "4",
        "Tx Bytes": "2368",
        "Tx Packets": "4"
    },
    {
        "AS Number": "",
        "AS Organization": "",
        "Address": "192.168.41.3",
        "Bytes": "3688",
        "City": "",
        "Country": "",
        "Latitude": "",
        "Longitude": "",
        "Packets": "8",
        "Rx Bytes": "2368",
        "Rx Packets": "4",
        "Tx Bytes": "1320",
        "Tx Packets": "4"
    }
]
