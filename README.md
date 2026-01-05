# jNetPcap SDK

[![Java](https://img.shields.io/badge/Java-22%2B-orange.svg)](https://openjdk.java.net/projects/jdk/22/) [![Maven Central](https://img.shields.io/badge/Maven-Central-blue.svg)](https://search.maven.org/artifact/com.slytechs.sdk/jnetpcap-sdk) [![License](https://img.shields.io/badge/License-Apache%20v2-green.svg)](https://www.apache.org/licenses/LICENSE-2.0)

**The easiest way to get started with jNetPcap.**

One dependency, everything included.

------

## Installation (Stable Release)

When the stable `3.0.0` is released:

```xml
<dependency>
    <groupId>com.slytechs.sdk</groupId>
    <artifactId>jnetpcap-sdk</artifactId>
    <version>3.0.0</version>
</dependency>
```

## Current Development Version (3.0.0-SNAPSHOT)

To use the latest development build (recommended for early adopters):

```xml
<repositories>
    <repository>
        <id>sonatype-snapshots</id>
        <url>https://central.sonatype.com/repository/maven-snapshots/</url>
        <releases><enabled>false</enabled></releases>
        <snapshots><enabled>true</enabled></snapshots>
    </repository>
</repositories>

<dependency>
    <groupId>com.slytechs.sdk</groupId>
    <artifactId>jnetpcap-sdk</artifactId>
    <version>3.0.0-SNAPSHOT</version>
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
import com.slytechs.sdk.protocol.tcpip.ip.Ip4;
import com.slytechs.sdk.protocol.tcpip.tcp.Tcp;

public class CaptureExample {
    public static void main(String[] args) throws PcapException {
        // Activate the community license (required, internet connection needed)
        NetPcap.activateLicense();
        
        // Pre-allocate headers for zero-allocation capture
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
}
```

------

## JVM Arguments

jNetPcap uses the Java Foreign Function & Memory (FFM) API for native libpcap access. Add these JVM arguments when running your application:

```bash
java --enable-native-access=com.slytechs.sdk.jnetpcap,com.slytechs.sdk.common -jar myapp.jar
```

For Maven exec plugin:

```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <configuration>
        <executable>java</executable>
        <arguments>
            <argument>--enable-native-access=com.slytechs.sdk.jnetpcap,com.slytechs.sdk.common</argument>
            <argument>-classpath</argument>
            <classpath/>
            <argument>com.example.MyApp</argument>
        </arguments>
    </configuration>
</plugin>
```

------

## License Activation

The jNetPcap SDK includes a **free unlimited Community Edition license**. To use it:

1. Call `NetPcap.activateLicense()` once at application startup
2. Internet connectivity is required for activation
3. No registration or API keys needed

```java
public static void main(String[] args) throws PcapException {
    // Required: Activate the community license before any capture operations
    NetPcap.activateLicense();
    
    // Now use jNetPcap normally
    try (var pcap = NetPcap.openOffline("capture.pcap")) {
        pcap.loop(-1, packet -> System.out.println(packet));
    }
}
```

The Community Edition is licensed under Apache 2.0 with telemetry. For commercial licenses without telemetry, contact [sales@slytechs.com](mailto:sales@slytechs.com).

------

## Optional Protocol Packs

Need more protocols? Add them (same version and repository rules apply):

```xml
<!-- Web protocols: HTTP, TLS, DNS, QUIC, WebSocket -->
<dependency>
    <groupId>com.slytechs.sdk</groupId>
    <artifactId>sdk-protocol-web</artifactId>
    <version>3.0.0-SNAPSHOT</version>
</dependency>

<!-- Infrastructure: BGP, OSPF, STP, VRRP, LACP, LLDP -->
<dependency>
    <groupId>com.slytechs.sdk</groupId>
    <artifactId>sdk-protocol-infra</artifactId>
    <version>3.0.0-SNAPSHOT</version>
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

## Examples

See the [jnetpcap-examples](https://github.com/slytechs-repos/jnetpcap-examples) repository for complete working examples:

- Live capture with BPF filters
- Offline pcap/pcapng file reading
- Protocol dissection (Ethernet, IPv4/6, TCP, UDP)
- Packet persistence and pooling
- Multi-threaded capture patterns
- Writing packets to files

------

## Documentation

- [jnetpcap-examples](https://github.com/slytechs-repos/jnetpcap-examples) - Working examples
- [jnetpcap-api README](https://github.com/slytechs-repos/jnetpcap-api) - Full API documentation
- [GitHub Wiki](https://github.com/slytechs-repos/jnetpcap-sdk/wiki) - User guides and tutorials
- [Javadocs](https://slytechs-repos.github.io/jnetpcap-api/) - API reference

------

## Gradle

```groovy
repositories {
    mavenCentral()
    maven { url 'https://central.sonatype.com/repository/maven-snapshots/' }
}

dependencies {
    implementation 'com.slytechs.sdk:jnetpcap-sdk:3.0.0-SNAPSHOT'
}
```

------

## License

Licensed under Apache License v2.0. See [LICENSE](https://github.com/slytechs-repos/jnetpcap-sdk/blob/main/LICENSE) for details.

------

**Sly Technologies Inc.** - High-performance network analysis solutions

Website: [www.slytechs.com](https://www.slytechs.com/)