# Frame_Relay_Multipoint

---

# Frame Relay Multipoint Network with Logical Diagram

This README provides a comprehensive explanation of the Frame Relay multipoint network setup, including the physical topology, logical design, and configuration details. The network demonstrates a Frame Relay WAN environment with interconnected routers in a hub-and-spoke architecture.

---

## **Overview**

### **Physical Topology**
The physical topology, as shown in the Cisco Packet Tracer screenshot, includes:
1. **Four Routers**:
   - **R0, R1, R2, R3**: Each connected via serial interfaces to a central Frame Relay cloud (`Cloud0`).
2. **Frame Relay Cloud**:
   - The cloud acts as a switching point for establishing virtual circuits (VCs).
3. **Serial Links**:
   - Each router connects to the cloud using Serial interfaces (S0/0 or S0/0/0).

### **Logical Diagram**
The logical diagram outlines:
1. **DLCI (Data Link Connection Identifier)**:
   - Each connection is represented by a unique DLCI assigned to routers.
   - **DLCI Values**:
     - Between R0 and R1: DLCI 101 and 401.
     - Between R0 and R2: DLCI 201 and 202.
     - Between R2 and R3: DLCI 301 and 302.
     - Between R1 and R3: DLCI 402.
2. **IP Addressing**:
   - The logical topology uses two subnets:
     - **10.1.0.0/29**: For interconnecting R0, R2, and R3.
     - **10.4.0.0/30**: For interconnecting R1 and R3.

---

## **Key Concepts**

### **What is Frame Relay?**
Frame Relay is a WAN technology that uses virtual circuits (VCs) to interconnect devices over a shared network. It provides cost-effective communication for bursty traffic by efficiently utilizing bandwidth.

### **Multipoint Configuration**
- **Multipoint Frame Relay**: A single physical interface can connect to multiple destinations using unique DLCIs.
- Routing decisions are based on DLCIs, allowing communication between all routers in the topology.

---

## **Network Configuration Details**

### **1. IP Addressing**
Each interface has been assigned IP addresses as follows:
- **10.1.0.0/29** for the main network connecting R0, R2, and R3.
- **10.4.0.0/30** for the network between R1 and R3.

### **2. Frame Relay DLCI Assignments**
- **DLCI Mapping**:
  - R0 ↔ R1: 101 (R0) ↔ 401 (R1)
  - R0 ↔ R2: 201 (R0) ↔ 202 (R2)
  - R2 ↔ R3: 301 (R2) ↔ 302 (R3)
  - R1 ↔ R3: 402 (R1) ↔ 102 (R3)

---

### **Router Configuration**

#### **Router R0 Configuration**
```bash
interface Serial0/0/0
 encapsulation frame-relay
 ip address 10.1.0.1 255.255.255.248
 frame-relay map ip 10.1.0.2 201 broadcast
 frame-relay map ip 10.1.0.3 101 broadcast
```

#### **Router R1 Configuration**
```bash
interface Serial0/0/0
 encapsulation frame-relay
 ip address 10.4.0.1 255.255.255.252
 frame-relay map ip 10.4.0.2 402 broadcast
 frame-relay map ip 10.1.0.3 401 broadcast
```

#### **Router R2 Configuration**
```bash
interface Serial0/0/0
 encapsulation frame-relay
 ip address 10.1.0.2 255.255.255.248
 frame-relay map ip 10.1.0.1 201 broadcast
 frame-relay map ip 10.1.0.4 301 broadcast
```

#### **Router R3 Configuration**
```bash
interface Serial0/0/0
 encapsulation frame-relay
 ip address 10.1.0.3 255.255.255.248
 frame-relay map ip 10.1.0.4 302 broadcast
 frame-relay map ip 10.4.0.1 102 broadcast
```

---

## **Verification**

### **1. Frame Relay Map**
Run the following command on any router to verify DLCI mappings:
```bash
show frame-relay map
```

### **2. Check Virtual Circuits**
To ensure the virtual circuits are established and operational:
```bash
show frame-relay pvc
```

### **3. Ping Test**
Use ping to verify connectivity between devices. For example:
```bash
ping 10.1.0.2
ping 10.4.0.2
```

---

## **Troubleshooting**

### **Common Issues**
1. **Misconfigured DLCI**:  
   Ensure that the DLCI matches the logical diagram configuration.
2. **Interface Status**:  
   Check if the serial interface is `UP/UP`. If not, verify cabling and encapsulation.
3. **Broadcasts Not Working**:  
   Add the `broadcast` keyword to Frame Relay maps to enable routing protocol advertisements.

### **Useful Commands**
- **View Interface Status**:
  ```bash
  show ip interface brief
  ```
- **Display Routing Table**:
  ```bash
  show ip route
  ```

---

## **Conclusion**

The provided Frame Relay topology demonstrates a robust multipoint setup with proper DLCI assignments, IP addressing, and interconnectivity verification. Both the physical Packet Tracer topology and the logical diagram align, showing the interconnection of R0, R1, R2, and R3 via a shared Frame Relay cloud.

By following the outlined configuration and verification steps, this setup can be replicated and customized for different WAN scenarios.

--- 


  
