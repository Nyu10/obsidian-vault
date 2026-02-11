

A byte is 8 bits
A ASCII character is a byte

2 power 

| **Power** | **Approximate value** | **Full name** | **Short name** |
| --------- | --------------------- | ------------- | -------------- |
| 10        | 1 Thousand            | 1 Kilobyte    | 1 KB           |
| 20        | 1 Million             | 1 Megabyte    | 1 MB           |
| 30        | 1 Billion             | 1 Gigabyte    | 1 GB           |
| 40        | 1 Trillion            | 1 Terabyte    | 1 TB           |
| 50        | 1 Quadrillion         | 1 Petabyte    | 1 PB           |

| **Operation name**                               | **Time**                |
| ------------------------------------------------ | ----------------------- |
| L1 cache reference                               | 0.5 ns                  |
| Branch mispredict                                | 5 ns                    |
| L2 cache reference                               | 7 ns                    |
| Mutex lock/unlock                                | 100 ns                  |
| Main memory reference                            | 100 ns                  |
| Compress 1K bytes with Zippy                     | 10,000 ns = 10 µs       |
| Send 2K bytes over 1 Gbps network                | 20,000 ns = 20 µs       |
| Read 1 MB sequentially from memory               | 250,000 ns = 250 µs     |
| Round trip within the same datacenter            | 500,000 ns = 500 µs     |
| Disk seek                                        | 10,000,000 ns = 10 ms   |
| Read 1 MB sequentially from the network          | 10,000,000 ns = 10 ms   |
| Read 1 MB sequentially from disk                 | 30,000,000 ns = 30 ms   |
| Send packet CA (California) -> Netherlands -> CA | 150,000,000 ns = 150 ms |

ns = nanosecond, 
µs = microsecond, 
ms = millisecond

1 ns = 10^-9 seconds
1 µs= 10^-6 seconds = 1,000 ns
1 ms = 10^-3 seconds = 1,000 µs = 1,000,000 ns

Memory is fast but the disk is slow.
• Avoid disk seeks if possible.
• Simple compression algorithms are fast.
• Compress data before sending it over the internet if possible