
# 📘 Networking Commands and NS-3 Practicals

---

## 🔧 Basic Networking Commands

| **Command** | **Description** |
|-------------|------------------|
| `ifconfig` | Configures and displays network interfaces |
| `ip` | Displays and manipulates routing, devices, and tunnels |
| `ip -v` | Shows version and output of IP commands |
| `ip addr` / `ip addr show` | Shows IP addresses and detailed configuration |
| `ip link` | Shows and configures network interfaces |
| `ip route show` | Displays the current routing table |
| `ifconfig -s` | Displays a short summary of network interfaces |
| `dig google.com` | Queries DNS servers for information |
| `nslookup google.com` | Interactively queries internet name servers |
| `netstat -at` | Displays all active TCP connections |
| `host google.com` | Performs DNS lookup to find IP address |
| `ping -c 7 google.com` | Sends ICMP echo requests to test connectivity |
| `sudo traceroute -T 184.95.56.34`<br>`sudo traceroute -T 127.0.0.53` | Traces the route packets take to reach a host |
| `route` | Displays or modifies the IP routing table |
| `arp` | Displays or modifies the ARP cache |
| `iwconfig` | Configures wireless network interfaces |
| `whois google.com` | Fetches domain registration information |

---

## 🧪 NS-3 Practicals

### ⚙️ Setup
Navigate to the NS-3 folder:
```bash
~/ns3/ns-allinone-3.35/ns-3.35
```
Open a terminal in the above directory.

---

### 🧪 Prac 2: Point-to-Point Topology
1. Copy `first.cc` to `scratch` folder.
2. Run:
   ```bash
   ./waf --run scratch/first
   ./waf --run scratch/first --vis
   ```

---

### 🧪 Prac 3: Bus Topology
1. Copy `second.cc` to `scratch` folder.
2. Run:
   ```bash
   ./waf --run scratch/second
   ./waf --run scratch/second --vis
   ```

---

### 🧪 Prac 4: Star Topology
1. Copy `star.cc` from `examples/tcp` to `scratch` folder.
2. Run:
   ```bash
   ./waf --run scratch/star
   ./waf --run scratch/star --vis
   ```

---

### 🧪 Prac 5: Mesh Topology
1. Copy `mesh.cc` from `src/mesh/examples` to `scratch` folder.
2. Run:
   ```bash
   ./waf --run scratch/mesh
   ./waf --run scratch/mesh --vis
   ```

---

### 🧪 Prac 6: Hybrid Topology (NetAnim)
1. Copy `third.cc` from `examples/tutorial` to `scratch` and rename it to `hybridanim.cc`.
2. Edit `hybridanim.cc`:
   - Add the following header files:
     ```cpp
     #include "ns3/netanim-module.h"
     #include "ns3/mobility-module.h"
     ```
   - Before `Simulator::Run();`, add:
     ```cpp
     AnimationInterface anim("hybridanim.xml");
     anim.SetConstantPosition(p2pNodes.Get(0), 10.0, 10.0);
     anim.SetConstantPosition(p2pNodes.Get(1), 20.0, 20.0);
     ```
3. Run:
   ```bash
   ./waf --run scratch/hybridanim
   ```
4. Open NetAnim:
   ```bash
   ./NetAnim
   ```
   - Load `hybridanim.xml` from:
     ```
     ns3/ns-allinone-3.35/ns-3.35/
     ```

---

### 🧪 Prac 7: UDP Client-Server
1. Copy `udp-client-server.cc` to `scratch`.
2. Run:
   ```bash
   ./waf --run scratch/udp-client-server
   ./waf --run scratch/udp-client-server --vis
   ```

---

### 🧪 Prac 8/9: TCP Bulk Send
1. Copy `Tcp.bulk.send.cc` to `scratch`.
2. Run:
   ```bash
   ./waf --run scratch/Tcp.bulk.send
   ./waf --run scratch/Tcp.bulk.send --vis
   ```

---

### 🧪 Prac 10: Wireshark
Analyze the traffic using **Wireshark** during any of the above simulations.
# 📡 Wireshark & Tshark Command Cheatsheet

## 🧠 Display Filters (Wireshark GUI or Tshark `-Y`)

| Purpose                        | Display Filter Example                          |
|-------------------------------|--------------------------------------------------|
| Filter by IP address          | `ip.addr == 192.168.1.10`                        |
| Filter by source IP           | `ip.src == 192.168.1.10`                         |
| Filter by destination IP      | `ip.dst == 192.168.1.20`                         |
| Filter by protocol            | `tcp`, `udp`, `icmp`, `http`, `dns`             |
| Filter by port (TCP/UDP)      | `tcp.port == 80`, `udp.port == 53`              |
| Show only HTTP traffic        | `http`                                          |
| Filter by MAC address         | `eth.addr == aa:bb:cc:dd:ee:ff`                 |
| Show only DNS queries         | `dns.flags.response == 0`                       |
| TCP packets with SYN flag     | `tcp.flags.syn == 1`                            |
| Packets with errors           | `tcp.analysis.flags`                            |

---

## 🖥️ Tshark (Command-Line Wireshark) Examples

### 🔍 Basic Capture
```bash
tshark -i eth0
```
Capture on interface `eth0`.

### 💾 Capture and Save to File
```bash
tshark -i eth0 -w output.pcap
```
Save the captured packets to a `.pcap` file.

### 🎯 Capture with Filter (e.g., only TCP)
```bash
tshark -i eth0 -f "tcp"
```
Using a **capture filter** (BPF syntax).

### 📄 Read from .pcap File
```bash
tshark -r file.pcap
```

### 🔍 Use a Display Filter (like Wireshark GUI)
```bash
tshark -r file.pcap -Y "ip.addr == 192.168.1.10"
```

### 📋 Print Specific Fields Only
```bash
tshark -r file.pcap -T fields -e ip.src -e ip.dst -e frame.len
```

### ⏱️ Limit the Number of Packets
```bash
tshark -c 100 -i eth0
```

---

## 🧠 Filter Types Summary

- **Capture Filters** (before capture, use with `-f`)
  - Example: `tcp port 80`, `host 192.168.1.10`, `udp`
- **Display Filters** (after capture, use with `-Y`)
  - Example: `ip.addr == 192.168.1.10`, `http.request.method == "GET"`

---

## 📘 Tips

- Use `tshark -D` to list all available interfaces.
- Combine filters to narrow down traffic:
  ```bash
  tshark -r capture.pcap -Y "http.request and ip.addr == 10.0.0.5"
  ```
- Use `-T json` or `-T fields` for structured output (great for scripting).

---


---

### 🧪 Prac 11: Flow Monitor in Point-to-Point
1. Copy `first.cc` to `scratch`.
2. Edit the file:
   - Add headers:
     ```cpp
     #include "ns3/flow-monitor.h"
     #include "ns3/flow-monitor-helper.h"
     #include "ns3/traffic-control-module.h"
     #include "ns3/ipv4-flow-classifier.h"
     ```
   - After:
     ```cpp
     clientApp.Stop(Seconds(10.0));
     ```
     add:
     ```cpp
     bool tracing = true;
     FlowMonitorHelper flowHelper;
     Ptr<FlowMonitor> monitor = flowHelper.InstallAll();
     ```
  
     add:
     ```cpp
     Simulator::Stop(Seconds(10.0));
     if (tracing) {
         pointToPoint.EnablePcapAll("p2p");
     }
     ```

 - Before:
     ```cpp
     Simulator::Run();
     ```
     
   - After simulation:
     ```cpp
     monitor->CheckForLostPackets();
     Ptr<Ipv4FlowClassifier> classifier = DynamicCast<Ipv4FlowClassifier>(flowHelper.GetClassifier());
     std::map<FlowId, FlowMonitor::FlowStats> stats = monitor->GetFlowStats();

     for (auto iter = stats.begin(); iter != stats.end(); ++iter) {
     Ipv4FlowClassifier::FiveTuple t = classifier->FindFlow(iter->first);
     double duration = iter->second.timeLastRxPacket.GetSeconds() - iter->second.timeFirstTxPacket.GetSeconds();
     double throughput = (iter->second.rxBytes * 8.0) / duration / 1000000.0;
     double avgDelay = (iter->second.rxPackets > 0) ? (iter->second.delaySum.GetSeconds() /iter->second.rxPackets) : 0;
  
     std::cout << "Flow " << iter->first << " (" << t.sourceAddress << " -> " <<t.destinationAddress << ")\n";
     std::cout << " Tx Packets: " << iter->second.txPackets << "\n";
     std::cout << " Rx Packets: " << iter->second.rxPackets << "\n";
     std::cout << " Packet Loss: " << (iter->second.txPackets - iter->second.rxPackets) << "\n";
     std::cout << " Delay Sum: " << iter->second.delaySum.GetSeconds() << " s\n";
     std::cout << " Response Time: " << avgDelay << " s\n";
     std::cout << " Dropped Packets: " << iter->second.lostPackets << "\n";
     std::cout << " Throughput: " << throughput << " Mbps\n";
     }
     
     ```
3. Run:
   ```bash
   ./waf --run scratch/first
   ```
