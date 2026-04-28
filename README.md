# **README**  
## **Introduction**  
This project aims to offer insights into DOS and DDOS attacks and defenses.  
## **Installation**  
To install the project, follow these steps:  
1. Clone the repository.  
2. Install the required dependencies.  
## **Usage**  
To run the application, use the following command:  
```bash  
python main.py  
```  
## **Contributing**  
We welcome contributions from the community.  
### **Steps to Contribute**  
1. Fork the repository.  
2. Create a branch for your feature.  
3. Submit a pull request.  
## **License**  
This project is licensed under the MIT License.


**DoS:** is an attack where a single source overwhelms a system (server, API, network, application) to make it unavailable to legitimate users. 
-When such an attack occurs, systems may become extremely slow, unresponsive, or completely unusable 

 

**DDoS Distributed Denial of Service attack:** 
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


1. What is DoS?

A Denial of Service (DoS) attack is when an attacker makes an application or system unavailable to legitimate users by overwhelming or abusing resources.

👉 Core idea:

Exhaust CPU
Exhaust Memory
Exhaust Threads / Connections
Exhaust Network bandwidth
Trigger expensive operations repeatedly
2. What happens in a DoS attack?

Think in terms of backend behavior:

Normal flow
User → Request → Server → Response
DoS flow
Attacker → Massive / Malicious Requests → Server overload → Legit users blocked
Impact:
Slow response
Application crash
Thread pool exhaustion
Database saturation
Service unavailable (HTTP 500 / timeout)
3. Types of DoS attacks (important for interviews + code review)
3.1 Volumetric (Network-level)
Flooding (UDP, ICMP)
Not visible in code review (infra level)
3.2 Application-level DoS (MOST IMPORTANT for you)

This is what you detect in SAST / code review

Examples:
Expensive DB queries
Infinite loops
Large payload processing
Regex backtracking (ReDoS)
File processing abuse
4. How to identify DoS in source code (SAST mindset)

You won’t see “DoS vulnerability” directly.
Instead, look for patterns that can be abused.

4.1 Unbounded loops / recursion
❌ Vulnerable
def process(data):
    while True:
        data = transform(data)

👉 If attacker controls input → infinite processing → CPU exhaustion

4.2 Missing rate limiting
@app.route("/login", methods=["POST"])
def login():
    return authenticate(request.json)

👉 No:

rate limit
lockout
throttling

➡️ Brute force → DoS

4.3 Large payload handling
data = request.get_json()
process(data)

👉 No size validation → attacker sends 100MB JSON

➡️ Memory exhaustion

4.4 Expensive database queries
String query = "SELECT * FROM orders";

👉 If:

large table
no pagination

➡️ Each request = heavy DB load

4.5 File upload abuse
file = request.files['file']
file.save("/uploads/" + file.filename)

👉 No:

size limit
type validation

➡️ Disk exhaustion

4.6 Regex DoS (ReDoS)
import re
re.match("(a+)+$", input)

👉 Crafted input → exponential backtracking
➡️ CPU spike

4.7 External calls without timeout (VERY IMPORTANT)
requests.get(user_url)

👉 No timeout:

threads hang
connection pool exhaustion

➡️ Leads to DoS

5. How to identify DoS (practical approach)
Step-by-step (like you do in Checkmarx triage)
Step 1: Identify user-controlled inputs
API payload
query params
headers
file uploads
Step 2: Trace execution

Check if input leads to:

loops
recursion
DB queries
file handling
regex
external calls
Step 3: Look for missing controls
❌ No rate limiting
❌ No input size restriction
❌ No timeout
❌ No pagination
❌ No caching
6. Real-world attack scenarios
Scenario 1: Login endpoint DoS
Attacker sends 10K login requests/sec
No rate limiting
➡️ Auth service crashes
Scenario 2: Search API abuse
GET /search?q=aaaaaaaaaaaaaaaaaaaaaaaa
Complex query
no optimization

➡️ DB CPU spike

Scenario 3: File upload DoS
Upload 5GB files repeatedly
➡️ Disk full → app failure
Scenario 4: SSRF → DoS combo
SSRF endpoint calls internal service repeatedly
➡️ Internal service crash
7. False positives (very important for SAST)
Case 1: Controlled loop
for i in range(10):
    process(i)

✅ Safe → bounded

Case 2: File upload with limits
if file.size > 5MB:
    reject()

✅ Safe

Case 3: External call with timeout
requests.get(url, timeout=3)

✅ Safe

8. Mitigation techniques (must know)
8.1 Rate limiting
Per IP
Per user

Example:

limit: 100 requests/min
8.2 Input size validation
if len(input) > 1000:
    reject()
8.3 Timeouts
requests.get(url, timeout=5)
8.4 Pagination (DB)
SELECT * FROM orders LIMIT 50 OFFSET 0;
8.5 Circuit breaker pattern
Stop repeated failures
Prevent cascading DoS
8.6 Resource limits
Max threads
Max connections
Memory caps
8.7 File upload restrictions
Size limit
Type validation
Storage quota
8.8 Regex protection
Avoid catastrophic patterns
Use safe regex engines
9. What happens if application is vulnerable?
Service downtime
Revenue loss
SLA violation
Customer impact
Possible lateral impact (microservices failure)
10. How to explain in interview (15+ yrs style)

“DoS in application security is not just about traffic flooding—it’s about identifying code paths where user input can trigger uncontrolled resource consumption. During SAST or manual review, I focus on patterns like unbounded loops, expensive DB operations, missing rate limits, and lack of timeouts. I also validate whether proper controls like pagination, throttling, and input size restrictions are implemented before marking it as exploitable.”

11. Quick checklist (for your daily standups)

When reviewing code, ask:

Is input size restricted?
Any unbounded loop/recursion?
Any expensive DB query?
Any file upload without limits?
Any external call without timeout?
Any missing rate limiting?


A Denial of Service (DoS) attack is a cybersecurity threat that targets the availability aspect of the CIA triad, meaning it aims to make a system, service, or network unavailable to legitimate users. When such an attack occurs, systems may become extremely slow, unresponsive, or completely unusable—often accompanied by symptoms like CPU utilization reaching 100% without a clear cause.

There are several types of DoS attacks, each with different techniques and impact. One type can be described as a highly targeted or “surgical” attack. In this scenario, an attacker sends a specially crafted packet designed to exploit a specific vulnerability, such as a buffer overflow or protocol weakness. This single malicious input can immediately crash or destabilize the system, effectively taking it down in one precise strike.

Another category is the resource exhaustion attack, often referred to as “death by a thousand cuts.” A classic example is the SYN flood attack. Here, the attacker repeatedly initiates TCP connections by sending SYN requests but never completes the handshake. By spoofing the source address, the server allocates resources and waits for responses that never arrive. Over time, these half-open connections accumulate, exhausting server resources and preventing legitimate users from connecting.

A more advanced and widely recognized form is the Distributed Denial of Service (DDoS) attack. In this case, the attacker controls a network of compromised systems, known as a botnet. These infected machines, often without their owners’ knowledge, simultaneously send massive volumes of traffic to the target system. Because the traffic originates from many distributed sources, it becomes extremely difficult to block or mitigate. This large-scale flooding overwhelms the system, leading to service disruption.

To defend against DoS and DDoS attacks, organizations implement multiple layers of protection. While the ideal solution—unlimited system capacity—is unrealistic, practical strategies include redundancy, where multiple systems are deployed to avoid single points of failure. Traffic pacing and rate limiting help control the volume of incoming requests, while ingress and egress filtering can block malicious or spoofed traffic.

Additionally, system hardening plays a crucial role by removing unnecessary services, disabling unused accounts, and reducing the attack surface. Regular patching and updates ensure known vulnerabilities are fixed. Continuous monitoring, using tools like SIEM and XDR, helps distinguish between legitimate traffic spikes and malicious activity. Finally, a well-defined incident response plan (SOAR) enables organizations to react quickly and effectively when an attack is detected.

In summary, DoS attacks come in various forms—from precise, single-packet exploits to large-scale distributed floods—but they all share the same goal: disrupting system availability. Effective defense requires a combination of proactive security practices, real-time monitoring, and rapid response capabilities.
 
