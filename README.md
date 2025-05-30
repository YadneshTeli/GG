
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
   - Before:
     ```cpp
     Simulator::Run();
     ```
     add:
     ```cpp
     Simulator::Stop(Seconds(10.0));
     if (tracing) {
         pointToPoint.EnablePcapAll("p2p");
     }
     ```
   - After simulation:
     ```cpp
     monitor->CheckForLostPackets();
     Ptr<Ipv4FlowClassifier> classifier = DynamicCast<Ipv4FlowClassifier>(flowHelper.GetClassifier());
     std::map<FlowId, FlowMonitor::FlowStats> stats = monitor->GetFlowStats();
     ```
3. Run:
   ```bash
   ./waf --run scratch/first
   ```
