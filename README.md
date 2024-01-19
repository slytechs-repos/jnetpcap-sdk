# jNetPcap SDK
A full development environment for network packet capture, analysis and transmission in Java.

## Overview
**jNetPcap SDK** is comprised of several java modules, each hosted on *Github*, that provides APIs for packet capture and transmission using the native *libpcap* library and its mechanism on many different platforms, see [*jNetPcap Wrapper*][jnetpcap-wrapper]. Once a packet is captured, each packet is further processed using [*jNetPcap API* ][jnetpcap-api] by going through a series of transnformers called **packet processors**.

The list of **packet processors** is extensive and more will be added in the future. Here is a short list of few top ones:
* `PacketDissector` - dissects raw packets and generates protocol information for use with [protocol packs][protocol-pack-sdk].
* `PacketPlayer` - Facilitates the playback of offline captures, allowing users to replay captured packets from a file at their original Inter-Frame Gap (IFG) or at a user-defined speed based on the embedded timestamps within each packet. This enables analysis and testing scenarios with precise control over the timing and pacing of network traffic.
* `DataObfuscator` - 4. DataObfuscator: Safeguards user privacy by employing obfuscation techniques to mask sensitive information within captured packets.

and many more.

## Examples
To get started lets take a look at a couple of examples.

Capturing and transmitting packets is straight forward and easy out of the box. 

> **Note**! **jNetPcap** also provides many useful utilities to help in working with the data received, such as byte arrays to hex string, and hex string to byte array, and much more. More advanced utility packet handlers such as no-copy on capture, are provides as well and discussed in the [*Wiki pages*][wiki]. 

### A one-liner or almost
This quick example[^1] demonstrates how to **read packets** from a file and print each packet contents (with all possible detail) to the console.

```java
try (var pcap = NetPcap.openOffline("pcaps/IPv4-ipf.pcapng")) {
	pcap.dispatch(System.out::println);
}
```
> View raw at [Example6 link][example6-link]

Which produces the following output:

```
Frame:      Frame #1, 192.168.1.225 > 142.250.81.238 proto=ICMPv4 len=1514 bytes info=ICMPv4
Frame:            Interface id = 0 (pcaps/IPv4-ipf.pcapng)     
Frame:            Frame Number = 1                             
Frame:            Arrival Time = 2023-04-19 23:01:47.079983000 [0x6440AB1B0001386F]
Frame:          Capture Length = 1514 bytes (12112 bits)       
Frame:            Frame Length = 1514 bytes (12112 bits)       
Frame:    
Ethernet:   + Ethernet II, src: ASUSTekC_34:47:63 dst: Verizon_f9:6f:a0 offset=0 length=14
Ethernet:  Destination Address = Verizon_f9:6f:a0 (20:C0:47:F9:6F:A0)
Ethernet:       Source Address = ASUSTekC_34:47:63 (54:04:A6:34:47:63)
Ethernet:                 Type = IPv4 (0x0800)                 
Ethernet: 
IPv4:       + Internet Protocol Version 4, offset=14, length=20
IPv4:                  Version = 4                             
IPv4:                            0100 .... = Version: 4        
IPv4:            Header Length = 5 [5*4 = 20 bytes]            
IPv4:                            .... 0101 = Header Length: 20 bytes (5)
IPv4:            Traffic class = 0x00 [DSCP: CS0, ECN: Not-ECT]
IPv4:                            0000 00.. = Differentiated Services Codepoint: Default (0)
IPv4:                            .... ..00 = Explicit Congestion Notification: Not ECN-Capable Transport (0)
IPv4:             Total Length = 1500 bytes                    
IPv4:           Identification = 0x7EF2 (32498)                
IPv4:                    Flags = 0x1 [MF]                      
IPv4:                            001. .... .... .... = Flags: 0x1 (••M)
IPv4:          Fragment Offset = 0 [0*8 = 0 bytes]]            
IPv4:                            ...0 0000 0000 0000 = Fragment Offset: 0
IPv4:             Time to Live = 64                            
IPv4:                 Protocol = ICMPv4 (1)                    
IPv4:          Header Checkusm = 0x32bd                        
IPv4:           Source Address = 192.168.1.225                 
IPv4:      Destination Address = 142.250.81.238                
IPv4:     
ICMPv4:     + Internet Control Message Protocol Echo (Ping), offset=34, length=8
ICMPv4:             Identifier = 26 (0x001A)                   
ICMPv4:        Sequence Number = 5                             
ICMPv4:
...
```

> **Note!** output trucated after the first packet for brevity

### Setup using Maven Central
	
```
<dependency>
    <groupId>com.slytechs.jnet.jnetpcap</groupId>
    <artifactId>jnetpcap-sdk</artifactId>
    <version>0.10.0</version>
</dependency>
```
## Contact
* `sales@slytechs.com` for commercial and licensing questions
* [*jNetPcap Issue Tracker*][bugs]

## Compatibility with jNetPcap version 1
There are API and license changes between version 1 and 2 of **jNetPcap**.
Please see [*wiki*][wiki] home page for details.

## Git Branches
So everyone is on the same page, we follow the following [branching model][git-branch-model].

> **Note** 'main' branch replaces old 'master' branch references in document as per [Github recommendation][why-master-deprecated].

[jnetpcap-api]: <https://github.com/slytechs-repos/jnetpcap-api>
[jnetpcap-wrapper]: <https://github.com/slytechs-repos/jnetpcap-wrapper>
[protocol-pack-sdk]: <https://github.com/slytechs-repos/protocol-pack-sdk>
[example6-link]: <https://raw.githubusercontent.com/slytechs-repos/jnetpcap-example/develop/src/main/java/com/slytechs/jnet/jnetpcap/example/Example6_smallest_footprint.java>

[jnetpcap_v1_page]: <https://sourceforge.net/projects/jnetpcap> "Legacy jNetPcap Version 1 Project Page"
[wiki]: <https://github.com/slytechs-repos/jnetpcap/wiki> "jNetPcap Project Wiki Pages"
[unit_test]: <https://github.com/slytechs-repos/jnetpcap/blob/main/src/test/java/org/jnetpcap/test/LibpcapApiTest.java> "jUnit Test of Main Libpcap API library"
[libpcap]: <https://www.tcpdump.org/> "This is the home web site of tcpdump, a powerful command-line packet analyzer; and libpcap, a portable C/C++ library for network traffic capture"
[npcap]: <https://npcap.com/> "Npcap is the Nmap Project's packet capture (and sending) library for Microsoft Windows"
[winpcap]: <https://www.winpcap.org/> "WinPcap is a library for link-layer network access in Windows environments"
[wireshark]: <https://wireshark.org> "Wireshark is the world’s foremost and widely-used network protocol analyzer"
[sf.net]: <https://sourceforge.net/projects/jnetpcap/> "jNetPcap version 1 hosted on SourceForge.net"
[bugs]: <https://github.com/slytechs-repos/jnetpcap-sdk/issues> "jNetPcap SDK bug reports on Github"
[homebrew]: <https://formulae.brew.sh/formula/libpcap> "Native libpcap install on Mac/Osx using Homebrew"
[macports]: <https://ports.macports.org/port/libpcap/> "Native libpcap install on Mac/Osx using Mac Ports"
[javadocs]: <https://slytechs-repos.github.io/jnetpcap/apidocs/org.jnetpcap/org/jnetpcap/package-summary.html> "jNetPcap v2 reference documentation"
[release]: <https://github.com/slytechs-repos/jnetpcap/releases/tag/v2.0.0-alpha.1> "Latest jNetPcap v2 release"
[jdk_matrix]: <https://www.java.com/releases/fullmatrix/> "JDK release full matrix"
[jep424]: <https://openjdk.org/jeps/424> "Foreign Function & Memory API (Preview)"
[git-branch-model]: <https://nvie.com/posts/a-successful-git-branching-model>
[why-master-deprecated]: <https://www.theserverside.com/feature/Why-GitHub-renamed-its-master-branch-to-main>
[jnetpcap-pro]: <https://github.com/slytechs-repos/jnetpcap-pro>
[core-protocols]: <https://github.com/slytechs-repos/core-protocols>
[download-bundle]: <https://github.com/slytechs-repos/slytechs-repos/releases>
[protocol-packs]: <https://github.com/slytechs-repos/jnetpcap-pro/wiki#about-protocol-packs>
[jnetworks]: http://slytechs.com/jnetworks
