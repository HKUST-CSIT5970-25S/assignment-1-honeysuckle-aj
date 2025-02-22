[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: Jiang Yihang
### Student Id: 21076351
### Email: yjiangeg@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance
wget https://phoronix-test-suite.com/releases/repo/pts.debian/files/phoronix-test-suite_10.8.4_all.deb
1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > - CPU performance test: compress-gzip, default params. reason: compress-gzip is a cpu performance test that measures the compression ability of CPUs. The compression process is a computationally intensive task that tackles multiple mathematical tasks and heavily relies on the processing power of the CPU. (I tried spec 2017 but it needs too much disk space so I gave up)
    > - Memory performance: pts/ramspeed, average(including Copy, Scale, Add and Triad), both Integer and Floating Point. It tests the comprehensive memory performance. 
2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance(s)(lower the better) | Memory performance(MB/s)(higher the better)<br />(Integer/Floating Point) |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` |      129.03           | 10393.99/failed |
    | `t2.medium`  |      52.85           |   19685.47/19627.88   |
    | `c5d.large` |        49.45         | 13355.18/13254.30 |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.



## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w        | RTT (ms) <br />min/avg/max/mdev(50 times) |
    | ------------------------- | -------------- | ----------------------------------------- |
    | `t3.medium` - `t3.medium` | 3.58 Gbits/sec | 0.226/0.289/0.925/0.109                   |
    | `m5.large` - `m5.large`   | 4.81 Gbits/sec | 0.175/0.187/0.206/0.005                   |
    | `c5n.large` - `c5n.large` | 9.47 Gbits/sec | 0.116/0.130/0.198/0.012                   |
    | `t3.medium` - `c5n.large` | 2.31 Gbits/sec | 0.652/0.697/0.951/0.051                   |
    | `m5.large` - `c5n.large`  | 2.81 Gbits/sec | 0.636/0.650/0.682/0.007                   |
    | `m5.large` - `t3.medium`  | 4.25 Gbits/sec | 0.197/0.223/0.345/0.022                   |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w        | RTT (ms)<br />min/avg/max/mdev(50 times) |
    | ------------------------- | -------------- | ---------------------------------------- |
    | N. Virginia - Oregon      | 33.6 Mbits/sec | 61.587/62.680/63.733/1.035               |
    | N. Virginia - N. Virginia | 4.68 Gbits/sec | 0.243/0.279/0.329/0.020                  |
    | Oregon - Oregon           | 4.96 Gbits/sec | 0.072/0.092/0.105/0.005                  |

    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.