# jNetPcap SDK

[![Java](https://img.shields.io/badge/Java-22%2B-orange.svg)](https://openjdk.java.net/projects/jdk/22/) [![Maven Central](https://img.shields.io/badge/Maven-Central-blue.svg)](https://search.maven.org/artifact/com.slytechs.sdk/jnetpcap-sdk) [![License](https://img.shields.io/badge/License-Apache%20v2-green.svg)](https://claude.ai/chat/LICENSE)

**The easiest way to get started with jNetPcap.**

One dependency, everything included.

------

## Installation

```xml
<dependency>
    <groupId>com.slytechs.sdk</groupId>
    <artifactId>jnetpcap-sdk</artifactId>
    <version>3.0.0</version>
</dependency>
```

That's it.

------

## What's Included

| Module             | Description                                   |
| ------------------ | --------------------------------------------- |
| jnetpcap-api       | High-level capture and analysis API           |
| jnetpcap-bindings  | Low-level libpcap FFM bindings                |
| sdk-protocol-tcpip | Ethernet, IPv4/6, TCP, UDP, VLAN, MPLS, IPsec |
| sdk-protocol-core  | Protocol dissection framework                 |
| sdk-common         | Memory management and utilities               |

------

## Quick Start

```java
import com.slytechs.jnet.jnetpcap.api.NetPcap;
import com.slytechs.jnet.protocol.tcpip.ip.Ip4;
import com.slytechs.jnet.protocol.tcpip.tcp.Tcp;

void main() throws PcapException {
    Ip4 ip4 = new Ip4();
    Tcp tcp = new Tcp();
    
    try (var pcap = NetPcap.openOffline("capture.pcap")) {
        
        pcap.dispatch(100, packet -> {
            
            if (packet.hasHeader(ip4)) {
                System.out.printf("IP: %s -> %s%n", ip4.src(), ip4.dst());
            }
            
            if (packet.hasHeader(tcp)) {
                System.out.printf("TCP: %d -> %d%n", tcp.srcPort(), tcp.dstPort());
            }
        });
    }
}
```

### Run

```bash
java --enable-native-access=com.slytechs.jnet.jnetpcap -jar myapp.jar
```

------

## Optional Protocol Packs

Need more protocols? Add them:

```xml
<!-- Web protocols: HTTP, TLS, DNS, QUIC, WebSocket -->
<dependency>
    <groupId>com.slytechs.sdk</groupId>
    <artifactId>sdk-protocol-web</artifactId>
    <version>3.0.0</version>
</dependency>

<!-- Infrastructure: BGP, OSPF, STP, VRRP, LACP, LLDP -->
<dependency>
    <groupId>com.slytechs.sdk</groupId>
    <artifactId>sdk-protocol-infra</artifactId>
    <version>3.0.0</version>
</dependency>
```

------

## Native Library Requirements

| Platform | Library | Installation                    |
| -------- | ------- | ------------------------------- |
| Linux    | libpcap | `apt install libpcap-dev`       |
| macOS    | libpcap | Pre-installed                   |
| Windows  | Npcap   | [npcap.com](https://npcap.com/) |

------

## Documentation

- [jnetpcap-api README](https://github.com/slytechs-repos/jnetpcap-api) - Full API documentation and examples
- [GitHub Wiki](https://github.com/slytechs-repos/jnetpcap-sdk/wiki) - User guides and tutorials
- [Javadocs](https://slytechs-repos.github.io/jnetpcap-api/) - API reference

------

## Gradle

```groovy
dependencies {
    implementation 'com.slytechs.sdk:jnetpcap-sdk:3.0.0'
}
```

------

## License

Licensed under Apache License v2.0. See [LICENSE](https://claude.ai/chat/LICENSE) for details.

------

**Sly Technologies Inc.** - High-performance network analysis solutions

Website: [www.slytechs.com](https://www.slytechs.com/)

------