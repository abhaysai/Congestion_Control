# Congestion_Control
This project compares the performance of three popular TCP congestion control algorithms—Cubic, BBR, and Vegas—across different network conditions. The experiments analyze throughput, latency (RTT), and packet loss to provide insights into each algorithm's strengths and weaknesses.
Project Overview
This experiment evaluates how different TCP congestion control mechanisms perform in varying network environments, specifically focusing on:

High bandwidth, low latency (50 Mbps, 10ms)
Low bandwidth, high latency (1 Mbps, 200ms)

# Prerequisites
Linux environment (tested on Ubuntu 20.04)
iproute2 package
iperf3
Python 3.6+ with matplotlib, pandas, and numpy
Root privileges for modifying congestion control settings

# Installation

# Install required packages:
bashsudo apt update
sudo apt install -y iperf3 python3-pip tc
pip3 install matplotlib pandas numpy

# Clone this repository:
bashgit clone https://github.com/yourusername/tcp-congestion-control-comparison.git
cd tcp-congestion-control-comparison


# Running the Experiments

# Step 1: Set Up Network Environment
The network_setup.sh script configures the network conditions using tc (traffic control):
bashsudo ./network_setup.sh
Example for high bandwidth, low latency scenario:
bashsudo ./network_setup.sh 50mbit 10ms 0% 100
Example for low bandwidth, high latency scenario:
bashsudo ./network_setup.sh 1mbit 200ms 0% 100

# Step 2: Run Tests for Each Algorithm
For each congestion control algorithm, run:
bashsudo ./run_test.sh
Examples:
bashsudo ./run_test.sh cubic 60 results_cubic_highbw.csv
sudo ./run_test.sh bbr 60 results_bbr_highbw.csv
sudo ./run_test.sh vegas 60 results_vegas_highbw.csv

# Step 3: Generate Visualizations
bashpython3 analyze_results.py --input-dir ./results --output-dir ./graph





# Key Findings
The expected results should show:

High Bandwidth, Low Latency (50 Mbps, 10ms):

Cubic: Avg Throughput ~46.8 Mbps, Avg RTT ~22.3 ms, Loss Rate ~0.21%
BBR: Avg Throughput ~48.2 Mbps, Avg RTT ~15.7 ms, Loss Rate ~0.08%
Vegas: Avg Throughput ~37.5 Mbps, Avg RTT ~12.2 ms, Loss Rate ~0.03%


Low Bandwidth, High Latency (1 Mbps, 200ms):

Cubic: Avg Throughput ~0.78 Mbps, Avg RTT ~352.6 ms, Loss Rate ~1.75%
BBR: Avg Throughput ~0.95 Mbps, Avg RTT ~253.8 ms, Loss Rate ~0.85%
Vegas: Avg Throughput ~0.91 Mbps, Avg RTT ~224.3 ms, Loss Rate ~0.12%



# Analysis Summary

Cubic: Most aggressive algorithm with highest loss rates, shows throughput spikes and RTT variation due to its loss-based approach that fills buffers until packet loss occurs.
BBR: Provides the best overall balance of metrics with high throughput and moderate RTT. Maintains more stable throughput by modeling the network path's bandwidth and RTT.
Vegas: Most latency-friendly with the lowest loss rates but typically at the cost of throughput in high-bandwidth scenarios. Performs relatively better in bandwidth-constrained environments.

# Troubleshooting

Permission Issues: Most commands require root privileges. Use sudo where necessary.
Congestion Control Not Available: Check available algorithms with:
bashcat /proc/sys/net/ipv4/tcp_available_congestion_control
You may need to load kernel modules:
bashsudo modprobe tcp_bbr
sudo modprobe tcp_vegas

iperf3 Connection Failures: Ensure no firewall is blocking localhost connections.
# Contributing
Contributions are welcome! Please feel free to submit a Pull Request.
