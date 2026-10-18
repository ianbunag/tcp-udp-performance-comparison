# TCP vs. UDP Performance Comparison

A discrete-event network simulation suite built with the **ns-3** simulator to evaluate and compare TCP and UDP transport-layer performance under varying network stress conditions.

> **Context:** Graduate Research Project — **CMPE-206: Computer Network Design** (May 2026)  
> **Researchers:** [Bhimsen Thapa Chhetri](https://github.com/bhimsenthapa1) & [John Ian Buñag](https://github.com/ianbunag)  
> **Technical Paper:** [TCP vs. UDP Performance Comparison (PDF)](./2026-05-10-final-report.pdf)  
> **Simulation Specifications & Data:** [simulations.md](./simulations.md)

Three empirical experiments were conducted across bottleneck dumbbell topologies—varying router buffer size, link latency (with a 0.0001% bit error rate), and TCP/UDP flow ratios—measuring throughput, mean delay, packet loss, retransmissions, duplicate ACKs, and Jain's Fairness Index.

## Install ns-3

https://www.nsnam.org

### Clone ns-3 with git
```bash
git clone https://gitlab.com/nsnam/ns-3-dev.git
cd ns-3-dev
git checkout -b ns-3.45-branch ns-3.45
```

### Clean ns-3 build
```bash
./ns3 clean
```

### Build ns-3
```bash
./ns3 configure --enable-examples --enable-tests
./ns3 build
```

### Verify ns-3 build
```bash
./ns3 run first
```

## Install netanim

### Clone netanim outside the ns-3 directory with git
```bash
git clone https://gitlab.com/nsnam/netanim.git
cd netanim
```

### Build netanim
```bash
mkdir build && cd build
cmake .. && cmake --build .
```

## Install Wireshark

https://www.wireshark.org/download.html

## Clone simulations repository

All commands below are run in the simulations' directory. ns-3 and netanim binaries are referenced with relative paths.

### Structure
- animations - contains netanim animation files
- monitors - contains PCAP and flow monitor XML files
- simulations - contains the C++ source code for the simulations
- project.cc - contains the main function that runs the simulations based on command line arguments

### Clone into ns-3 scratch directory with git
```bash
cd ns-3-dev/scratch
git clone git@github.com:ianbunag/tcp-udp-performance-comparison.git
cd tcp-udp-performance-comparison
```

### Run a simulation
```bash
./../../ns3 run 'project --simulation=example'
```

> See simulations directory for available simulations.

### View animations
```bash
../../../netanim/build/netanim
```

> Open the .xml file in the `animations` directory corresponding to your simulation to view the animation.

### Gather metrics

#### Throughput, Mean Delay, and Packet Loss Ratio
```bash
python3 ../../src/flow-monitor/examples/flowmon-parse-results.py monitors/<filename>.xml
```

> Jitter sum and rxPackets can be located in the flow monitor XML file.

#### Retransmission and duplicate ACK counts

1. Open Wireshark
2. Open the .pcap file in the `monitors` directory corresponding to your simulation.
3. Click Analyze > Expert Information to view the retransmission and duplicate ACK counts.

## Adding a new simulation
1. Create a new file in `simulations` directory with the name of your simulation, e.g. `my-simulation.cc`.
2. Add your simulation code to the file with a unique function name, e.g. `void runMySimulation()`.
3. Set the log component name within the function, e.g. `NS_LOG_COMPONENT_DEFINE("my-simulation");`.
4. Add a new case to the `project` function in `project.cc` to run your simulation when the `--simulation` argument is set to the name of your simulation, e.g. `my-simulation`.
5. Build and run your simulation using the command above, replacing `example` with the name of your simulation, e.g. `my-simulation`.
