The **OSI (Open Systems Interconnection) model** is a conceptual framework that standardizes the functions of a telecommunication or computing system into seven distinct layers. It describes how network hardware and software work together to transmit data. While the modern internet is based on the TCP/IP model, the OSI model is still a valuable tool for understanding network communication, troubleshooting, and developing network technologies.

Here are the seven layers of the OSI model, from bottom to top:

**1. Physical Layer (Layer 1):**

* **Function:** This layer deals with the physical connection between network devices. It's responsible for the transmission and reception of raw data as electrical, radio, or optical signals over a physical medium (e.g., cables, fiber optics, wireless).
* **Key Aspects:**
    * Physical media and connectors
    * Voltage levels, timing of signals
    * Data transmission rates
    * Physical network topologies (e.g., bus, star, ring, mesh)
    * Transmission modes (simplex, half-duplex, full-duplex)
* **Examples:** Ethernet cables, fiber optic cables, radio frequencies, hubs, repeaters.

**2. Data Link Layer (Layer 2):**

* **Function:** This layer provides reliable data transfer between two directly connected nodes over the physical layer. It packages raw bits from the physical layer into frames and handles physical addressing (MAC addresses), error detection, and flow control within a local network.
* **Key Aspects:**
    * Framing of data
    * Physical addressing (MAC addressing)
    * Error detection and correction within the local link
    * Flow control
    * Media Access Control (MAC) and Logical Link Control (LLC) sublayers
* **Examples:** Ethernet, Wi-Fi, network switches, MAC addresses.

**3. Network Layer (Layer 3):**

* **Function:** This layer is responsible for routing data packets across multiple networks. It handles logical addressing (IP addresses), determines the best path for data to travel (routing), and manages network congestion.
* **Key Aspects:**
    * Logical addressing (IP addressing: IPv4 and IPv6)
    * Routing protocols (e.g., RIP, OSPF, BGP)
    * Packet forwarding
    * Fragmentation and reassembly of packets
    * Quality of Service (QoS)
* **Examples:** Routers, IP protocol, ICMP.

**4. Transport Layer (Layer 4):**

* **Function:** This layer provides end-to-end communication between applications running on different hosts. It ensures reliable and ordered data delivery, handles segmentation and reassembly of data, and provides flow and error control at the transport level.
* **Key Aspects:**
    * Segmentation and reassembly of data
    * Connection-oriented (e.g., TCP) and connectionless (e.g., UDP) communication
    * Reliable data transfer (with acknowledgements and retransmissions in TCP)
    * Flow control
    * Port numbers for application identification
* **Examples:** TCP, UDP.

**5. Session Layer (Layer 5):**

* **Function:** This layer manages the establishment, maintenance, and termination of connections (sessions) between applications. It handles synchronization, dialogue control, and checkpointing of data streams.
* **Key Aspects:**
    * Session establishment and termination
    * Session management (keeping connections active)
    * Synchronization of data transfer
    * Dialogue control (who can transmit at what time)
    * Checkpointing and recovery
* **Examples:** NetBIOS, SIP, H.323.

**6. Presentation Layer (Layer 6):**

* **Function:** This layer is responsible for data formatting, encryption, and compression to ensure that data is in a usable format for the application layer and can be understood by the receiving system. It acts as a translator between different data formats.
* **Key Aspects:**
    * Data formatting and conversion
    * Data encryption and decryption (e.g., SSL/TLS)
    * Data compression and decompression
    * Character encoding (e.g., ASCII, UTF-8)
* **Examples:** JPEG, MPEG, SSL/TLS.

**7. Application Layer (Layer 7):**

* **Function:** This is the layer closest to the end-user and provides network services directly to applications. It includes protocols that applications use to communicate with the network and with each other.
* **Key Aspects:**
    * Providing network services to applications
    * User interface to network services (indirectly)
    * Specific protocols for different applications
* **Examples:** HTTP, FTP, SMTP, DNS, POP3, IMAP.

![image](https://github.com/user-attachments/assets/0c51a049-be92-4df7-9f35-1457af31953d)
