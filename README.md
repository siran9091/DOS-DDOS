# DOS-DDOS
DoS: is an attack where a single source overwhelms a system (server, API, network, application) to make it unavailable to legitimate users.
-When such an attack occurs, systems may become extremely slow, unresponsive, or completely unusable 

 

**DDoS Distributed Denial of Service attack: **
a group of malicious nodes floods a server with so many requests that it can't handle them all. (botnets, compromised devices, cloud instances). 

This overload makes the server inaccessible for genuine users trying to access it normally.  

The goal is to disrupt the server's operations, causing downtime or slowdowns that can impact businesses and users.  

Denial of Service attacks use different techniques like flooding the server with traffic or overwhelming it with requests, making them difficult to stop once they start.  

 

Attacker’s Goal 

Service disruption (downtime)  

💰 Financial damage (lost revenue, SLA penalties)  

🧠 Reputation damage  

⚠️ Extortion (“Pay or we keep attacking”) 

 

While DoS is ongoing, attacker performs: 

Privilege escalation from a DoS (Denial of Service) attack occurs when the system, overwhelmed or crashed by the attack, fails into an insecure state, allowing attackers to bypass authentication or gain elevated administrative permissions. Common vectors include exploiting software vulnerabilities (like buffer overflows), leveraging automated service restarts, or abusing security misconfigurations caused by the crash. 
 
Buffer Overflow Exploits: Attackers often use a DoS attack to cause a buffer overflow (flooding a program with too much data). If this overflows into a memory space that controls user privileges, it can be manipulated to execute code with root/admin rights. 

A Denial of Service (DoS) attack is a cybersecurity threat that targets the availability aspect of the CIA triad, meaning it aims to make a system, service, or network unavailable to legitimate users. When such an attack occurs, systems may become extremely slow, unresponsive, or completely unusable—often accompanied by symptoms like CPU utilization reaching 100% without a clear cause.

There are several types of DoS attacks, each with different techniques and impact. One type can be described as a highly targeted or “surgical” attack. In this scenario, an attacker sends a specially crafted packet designed to exploit a specific vulnerability, such as a buffer overflow or protocol weakness. This single malicious input can immediately crash or destabilize the system, effectively taking it down in one precise strike.

Another category is the resource exhaustion attack, often referred to as “death by a thousand cuts.” A classic example is the SYN flood attack. Here, the attacker repeatedly initiates TCP connections by sending SYN requests but never completes the handshake. By spoofing the source address, the server allocates resources and waits for responses that never arrive. Over time, these half-open connections accumulate, exhausting server resources and preventing legitimate users from connecting.

A more advanced and widely recognized form is the Distributed Denial of Service (DDoS) attack. In this case, the attacker controls a network of compromised systems, known as a botnet. These infected machines, often without their owners’ knowledge, simultaneously send massive volumes of traffic to the target system. Because the traffic originates from many distributed sources, it becomes extremely difficult to block or mitigate. This large-scale flooding overwhelms the system, leading to service disruption.

To defend against DoS and DDoS attacks, organizations implement multiple layers of protection. While the ideal solution—unlimited system capacity—is unrealistic, practical strategies include redundancy, where multiple systems are deployed to avoid single points of failure. Traffic pacing and rate limiting help control the volume of incoming requests, while ingress and egress filtering can block malicious or spoofed traffic.

Additionally, system hardening plays a crucial role by removing unnecessary services, disabling unused accounts, and reducing the attack surface. Regular patching and updates ensure known vulnerabilities are fixed. Continuous monitoring, using tools like SIEM and XDR, helps distinguish between legitimate traffic spikes and malicious activity. Finally, a well-defined incident response plan (SOAR) enables organizations to react quickly and effectively when an attack is detected.

In summary, DoS attacks come in various forms—from precise, single-packet exploits to large-scale distributed floods—but they all share the same goal: disrupting system availability. Effective defense requires a combination of proactive security practices, real-time monitoring, and rapid response capabilities.

 
