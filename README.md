# jNetPcap SDK

[![Java](https://img.shields.io/badge/Java-22%2B-orange.svg)](https://openjdk.java.net/projects/jdk/22/) [![Maven Central](https://img.shields.io/maven-central/v/com.slytechs.sdk/jnetpcap-sdk.svg)](https://central.sonatype.com/artifact/com.slytechs.sdk/jnetpcap-sdk) [![License](https://img.shields.io/badge/License-Apache%20v2-green.svg)](https://www.apache.org/licenses/LICENSE-2.0)

**The easiest way to get started with jNetPcap.**

One dependency, everything included.

------

## Installation

```xml
<dependency>
    <groupId>com.slytechs.sdk</groupId>
    <artifactId>jnetpcap-sdk</artifactId>
    <version>3.0.0</version>
    <type>pom</type>
</dependency>
```

### Latest Development Build (3.1.0-SNAPSHOT)

```xml
<repositories>
    <repository>
        <id>central-portal-snapshots</id>
        <url>https://central.sonatype.com/repository/maven-snapshots/</url>
        <releases><enabled>false</enabled></releases>
        <snapshots><enabled>true</enabled></snapshots>
    </repository>
</repositories>

<dependency>
    <groupId>com.slytechs.sdk</groupId>
    <artifactId>jnetpcap-sdk</artifactId>
    <version>3.1.0-SNAPSHOT</version>
    <type>pom</type>
</dependency>
```

------

## What's Included

| Module               | Description                                    |
| -------------------- | ---------------------------------------------- |
| `jnetpcap-api`       | High-level capture and protocol dissection API |
| `jnetpcap-bindings`  | Low-level libpcap FFM bindings (1:1 libpcap)   |
| `sdk-protocol-core`  | Packet, Header, dissection framework           |
| `sdk-protocol-tcpip` | Ethernet, IPv4/IPv6, TCP, UDP, ICMP and more   |
| `sdk-protocol-web`   | HTTP, TLS, QUIC, HTTP/2, HTTP/3                |
| `sdk-common`         | Memory management, pooling, utilities          |

------

## Quick Start

```java
import com.slytechs.sdk.jnetpcap.api.NetPcap;
import com.slytechs.sdk.protocol.tcpip.ip.Ip4;
import com.slytechs.sdk.protocol.tcpip.tcp.Tcp;

try (var pcap = NetPcap.openOffline("capture.pcap")) {
    Ip4 ip4 = new Ip4();
    Tcp tcp = new Tcp();

    pcap.dispatch(100, packet -> {
        if (packet.hasHeader(ip4))
            System.out.printf("IP: %s -> %s%n", ip4.src(), ip4.dst());

        if (packet.hasHeader(tcp))
            System.out.printf("TCP: %d -> %d%n", tcp.srcPort(), tcp.dstPort());
    });
}
```

No license calls required — the Community Edition activates automatically.

### Run

```bash
java --enable-native-access=com.slytechs.sdk.jnetpcap,com.slytechs.sdk.common \
     -jar myapp.jar
```

------

## Live Capture

```java
try (var pcap = NetPcap.create("eth0")) {
    pcap.setSnaplen(65535)
        .setPromisc(true)
        .setTimeout(Duration.ofSeconds(1))
        .activate();

    pcap.setFilter("tcp port 443");

    Ip4 ip4 = new Ip4();
    pcap.loop(-1, packet -> {
        if (packet.hasHeader(ip4))
            System.out.println(ip4.src() + " -> " + ip4.dst());
    });
}
```

------

## Optional Protocol Packs

```xml
<!-- Web protocols: HTTP, TLS, QUIC, HTTP/2, HTTP/3 -->
<dependency>
    <groupId>com.slytechs.sdk</groupId>
    <artifactId>sdk-protocol-web</artifactId>
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

## Gradle

```groovy
repositories {
    mavenCentral()
    // For snapshots only:
    maven { url 'https://central.sonatype.com/repository/maven-snapshots/' }
}

dependencies {
    implementation 'com.slytechs.sdk:jnetpcap-sdk:3.0.0'
}
```

------

## Examples

See [jnetpcap-examples](https://github.com/slytechs-repos/jnetpcap-examples) for working examples covering:

- Live capture and offline file reading
- Protocol dissection (Ethernet, IP, TCP, HTTP, TLS)
- BPF filtering
- Packet dumping with `PcapDumper`
- Pooled zero-allocation capture
- Multi-threaded producer-consumer patterns
- Interface enumeration

------

## Documentation

- [API Specification](https://github.com/slytechs-repos/jnetpcap-api) — Full API reference
- [Javadocs](https://slytechs-repos.github.io/jnetpcap-api/) — Generated API docs
- [GitHub Wiki](https://github.com/slytechs-repos/jnetpcap-sdk/wiki) — Guides and tutorials

------

## License

Licensed under the [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0).

The Community Edition includes telemetry. For commercial licenses without telemetry, contact [sales@slytechs.com](mailto:sales@slytechs.com).

------

**[Sly Technologies Inc.](https://www.slytechs.com/)** — High-performance network analysis solutions