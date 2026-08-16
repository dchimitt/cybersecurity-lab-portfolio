# Lab Setup

## Overview

This lab establishes an isolated virtual network for cybersecurity testing and experimentation. It consists of three virtual machines: Kali Linux (KL), Windows 10 (W10), and Metasploitable 2 (M2).

KL serves as my security testing machine, while W10 and M2 will serve as target systems for future labs as this portfolio progresses. Each of the target systems is connected to an isolated virtual network that allows communication with KL and each other. More importantly, the virtual network was set up such that these target VMs do not have access to the internet.

KL uses two network interfaces to separate its roles in this project. The first interface provides internet access to KL so I may download any needed tools. The second interface connects KL to both target VMs through an isolated testing network.

This lab serves as the foundation for all aspects of this portfolio including network reconnaissance, vulnerability assessment, exploitation, and analysis.

## Objectives

1. Establish a functional and realistic cybersecurity testing environment containing different operating systems.
2. Create an isolated virtual network and ensure it allows connectivity between the three VMs without compromising my home system.
3. Provide internet access to KL through a separate network that does not interact with the target VMs.
4. Verify connectivity between all VMs and confirm that target VMs do not have access to the internet.
5. Document the network architecture, configurations, and connectivity test results to provide a reliable foundation for future labs.

## Network Architecture

This lab is divided into two separate virtual networks. VMnet8 provides internet access to KL through a NAT connection, while VMnet1 provides an isolated testing network for communication between KL and the two target VMs.

### VMnet8 -- NAT Network

VMnet8 uses the 192.168.149.0/24 network and provides KL with internet access through NAT. KL connects to this network through its eth1 interface, which is assigned the IP address 192.168.149.130.

This interface is used by KL for activities that require internet access, such as downloading tools I may need as my portfolio progresses. Rather than deciding what is needed upfront, I created VMnet8 to allow myself flexibility as I discover new technologies or come up with ideas to improve this repo.

### VMnet1 -- Isolated Testing Network

VMnet1 uses the 192.168.204.0/24 network and serves as the isolated network for all cybersecurity testing. All three VMs are connected to this network. The assigned addresses are as follows:

| Virtual Machine | Interface | IP Address |
|---|---|---|
| KL | `eth0` | `192.168.204.128` |
| W10 | -- | `192.168.204.129` |
| M2 | -- | `192.168.204.130` |

## Connectivity Testing

Connectivity tests were performed to verify that the three VMs could communicate across the isolated testing network while the target VMs remained unable to access the internet. The expected network behavior was:

| Test | Expected Result |
|---|---|
| KL --> W10 | Successful |
| KL --> M2 | Successful |
| W10 --> Kali | Successful |
| W10 --> M2 | Successful |
| M2 --> KL | Successful |
| M2 --> W10 | Successful |
| KL --> Internet via `eth1` | Successful |
| W10 --> Internet | Blocked |
| M2 --> Internet | Blocked |

The KL connectivity tests confirmed that it could communicate with both target VMs through `eth0` while maintaining internet connectivity through `eth1`.

The W10 connectivity tests confirmed communication with the other two VMs through the isolated VMnet1 interface, while also demonstrating it did not have internet access.

The M2 connectivity tests mirrored the W10 connectivity tests.

Test results matched the expected results. All three VMs were able to communicate across the isolated testing network, while only KL retained internet access through its separate NAT interface.

## Security Considerations

The network architecture was designed to reduce the risk associated with using intentionally vulnerable systems in a cybersecurity testing sandbox environment.

M2 is intentionally vulnerable  and is being used as one of the target VMs in these labs. By keeping it isolated to the VMnet1 network, it is prevented from having direct access to the internet but it can still communicate with both KL and W10 for testing purposes.

W10, while a much more secure operating system, no longer receives updates and is being phased out. To adhere to best practices, I also omitted internet access from this VM.

KL was given access to the internet through a separate network interface connecting through VMnet8/NAT. As I do not know the full scope of this portfolio as I start it, I feel it is important to give myself the ability to download any tools as the need arises.

## Evidence

Supporting evidence for this lab is organized into the following directories:

- network-diagram/ -- Final network architecture diagram in both .drawio and .png formats.
- network-configuration/ -- Screenshots documenting the network adapter configurations and IP addressing of each VM.
- connectivity-tests/ -- Screenshots documenting connectivity between the VMs and vertification of internet isolation for the target VMs.

## Lessons Learned

This lab gave me a better understanding of how virtual networking can be used to create a controlled sandbox testing environment. I learned how to configure multiple network adapters on separate VMs and how they can be used for different purposes.

I also learned the importance of confirming results through terminals rather than simply trusting they work based on my initial settings. The adapter settings I originally used seemed to work until I actually performed connectivity tests between each VM. By running tests, I was able to confirm that the target VMs were actually isolated from the internet which was my top security consideration in this lab.

This lab gave me much needed experience in working with other operating systems, especially Linux systems, which I did not have as much exposure to. This also helped me become more comfortable working in the terminal and using commands to navigate directories, create files, and work with git.

## Next Steps

This lab provides a baseline environment to perform cybersecurity testing in a controlled environment. Future labs may include network reconnaissance, vulnerability assessment, exploitation, detection, and security analysis. 
