---
created: 2023-09-10T00:07:34 (UTC -04:00)
tags: 
source: https://medium.com/dnsfilterofficial/how-a-c2-server-is-leveraged-in-a-botnet-command-and-control-attack-42d03da1e5f5
author: Fikayo Adepoju
dg-publish: true
---

> [!abstract] 
>  A Command and Control attack is a component of a malware attack used to establish a remote covert channel between a compromised host and the attacker’s server. The attacker’s server is often referred…


---
[

![Fikayo Adepoju](https://miro.medium.com/v2/resize:fill:88:88/2*mHJjRM8s4mDCGvxAigFAlw.jpeg)



](https://medium.com/@coderonfleek?source=post_page-----42d03da1e5f5--------------------------------)[

![DNSFilter Blog](https://miro.medium.com/v2/resize:fill:48:48/1*7wWJgWTgQfEaCxbUdn-3bA.png)



](https://medium.com/dnsfilterofficial?source=post_page-----42d03da1e5f5--------------------------------)

## What is a Command and Control Attack?

A Command and Control attack is a component of a malware attack used to establish a remote covert channel between a compromised host and the attacker’s server. The attacker’s server is often referred to as a Command and Control server, C2 server, or C & C server. This attack server exploits the backdoor created to carry out different types of malicious activities on the victim’s computer, for example, data exfiltration through [DNS tunneling](https://www.dnsfilter.com/blog/dns-tunneling-malware).

Command and Control (C2) servers often take advantage of trusted and rarely monitored traffic like DNS to send commands back to the infected host.

Given enough time, the malware can identify and spread to more vulnerable hosts on the network, a process known as _pivoting_. With more systems recruited, a network of zombie systems (botnet) is created. These botnets then receive commands to perform more damaging activities, for example, launching Distributed Denial of Service attacks.

## How do Command and Control (C2) servers work?

Command and Control attacks can be achieved in a series of steps described below:

**Step 1:**

The attacker infects a user’s system or a system within an organization (often behind a firewall) with malware. This can be done using different methods like phishing emails, [malvertising](https://www.dnsfilter.com/blog/malvertising-is-on-the-rise), vulnerable browser plugins, or direct installation of malicious software through a USB stick or disc drive, etc.

![](https://miro.medium.com/v2/resize:fit:1400/0*DfP8O3M197Nvss7y.png)

**Step 2:**

Once the host is infected, the C2 channel is created and the compromised system sends back an acknowledgment to the C2 server to indicate that it is ready to receive commands. This communication is mostly done over trusted traffic like DNS.

![](https://miro.medium.com/v2/resize:fit:1400/0*OExNPqhL_9fRpEAP.png)

**Step 3:**

With the C2 channel established, the infected system can now receive further commands from the C2 server as long as the malware stays undetected. The Command and Control server can use this channel to send more malicious software to be installed, encrypt data, and even recursively extract data from the infected host.

![](https://miro.medium.com/v2/resize:fit:1400/0*JdfEfmMot7d9kHYi.png)

The C2 server can also issue commands to the infected host to begin looking for vulnerabilities on other systems in the network in order to pivot through the network. This can completely compromise the whole IT infrastructure of an organization and lead to the creation of a network of compromised hosts known as a botnet. The sole purpose of this entire organization of zombie machines is to receive commands from the Command and Control server to perform coordinated attacks.

## Types of Malware Command and Control Techniques

There are different models that Command and Control attacks employ to send commands to infected systems. The inspiration behind the different command-issuing models is to make it more difficult to detect where the commands are originating from.

**Centralized Model:** This model is very similar to a client-server transaction model. An infected host can poll its C2 server to request for commands on operations to be executed. It’s very common for attackers to use common websites and hosting services to host C2 servers.

This model is often easy to detect and block because the commands come from a single source with an IP that can be quickly detected and blocked.

Some attackers, however, make the detection process more difficult by using proxies, redirectors, and load balancers in their C2 server setup.

**Peer-to-Peer Model:** This adopts a decentralized model with members of a botnet passing messages from one node to another. This eliminates the need for a “main” or central server making it more difficult to detect.

Even when detected, you might only be able to take down one node at a time. This model is often used with the centralized model as a hybrid setup. In this hybrid scenario, the P2P communication serves as a fallback when the central server is taken down.

**Random:** This model was developed to ensure that cybersecurity experts cannot detect the chain of command of a botnet or trace and shut down the C2 server. This is achieved by sending commands to the infected host or botnet from different random sources. These sources could be links in social media comments, CDNs, Gmail, IRC chat rooms, and many more rented mediums. One common attribute attackers look for in these sources is that people mostly trust and use them frequently.

## How a Command and Control server can be used to exploit victims

**Data theft:** One of the common uses of a C2 channel is for data exfiltration. Using the backdoor created, attackers can recursively collect sensitive data from an organization’s system. This sensitive data could be financial records, trade secrets, confidential information that can be used for insider trading, or other high-value data that would turn a large profit on the darkweb.

**Disrupt ongoing tasks:** Attackers can periodically restart an infected system or group of compromised systems within an organization when an important task is going on. For example, a company performing data migration or backing up its entire database of information and files to multiple external storage systems for redundancy. Or a manufacturing process that involves the processing of materials from one form to another and could take several long hours or days. Attackers can repeatedly interrupt this process by restarting the system(s) at timed intervals so that the task will never be completed.

**Shutdown systems or networks:** In a case where the malware has been able to pivot through either a portion of or all systems within an organization, an attacker can simply shut down all the systems to completely terminate business operations. The attacker can then threaten that business activities will not resume until their needs are met. This is a form of ransomware that bypasses encryption.

**Distributed Denial of Service attacks:** Sometimes, the infected systems are not the actual target and are only used as foot soldiers to initiate attacks on a different organization or service. An attacker in control of a C2 server that issues commands to a set of zombie systems can instruct the infected systems to overwhelm a server (e.g. Facebook’s server) with numerous requests to exhaust the target’s bandwidth.

This then leads to legitimate users being denied access to the website or service.

**Data Encryption or Corruption:** Another damaging activity a Command and Control server can instruct a host to perform is to encrypt or corrupt files on the system. This can be used to request ransom before the data is released or simply to sabotage an organization’s activities.

The above are just a few examples of the common attacks that can be performed through a Command and Control server. C2 channels are a backdoor to the infected computer and/or network and their damage potential is only limited by the attacker’s mind.

## How to detect and prevent Command and Control attacks

**Scan and filter all traffic:** This is the **most important** precaution an organization can take to prevent and [detect Command and Control activities](https://help.dnsfilter.com/hc/en-us/articles/1500008113221-Locating-Query-Source). Both inbound and outbound traffic needs to be monitored to detect suspicious activities like unauthorized encryption of network traffic (commonly used in [DNS tunneling](https://www.dnsfilter.com/blog/dns-tunneling-malware) operations), traffic to unrecognized servers, etc.

A DNS filtering solution like DNSFilter can act as the gatekeeper for all of your DNS traffic — that includes DNS traffic both into and out of your organization. C2 covert channels can be prevented from being set up and/or used when you rely on an AI-driven solution that dynamically adds malicious and suspicious domains as they are uncovered to a block list.

You can start with a [free DNSFilter account](https://app.dnsfilter.com/signup) today to guard yourself against attacks that leverage Command and Control servers.

**Enforce an “allow” list of applications that can be installed on the system:** Your computer system should not allow just _any_ application to be run or installed. By blocking the installation of applications from unknown sources, you can prevent malware used to create C2 channels from being installed on your system.

**Separate infected hosts from other hosts on the network:** When an infected computer is detected, remove it immediately from the network to isolate it from other hosts. This prevents the malware from pivoting and taking control of the network.

**Constantly scan your system with antivirus software:** Using battle-tested antivirus software, you can constantly scan your host systems to ensure that malware activities are detected and removed. This way, the malware used in communicating with C2 servers is removed, hence terminating the covert channel.

## Conclusion

Command and Control attacks are carried out in a stealthy manner to ensure that the attacker isn’t prevented from carrying out their malicious activities and that the organization can’t mitigate the damage. Thus, it is important to prevent malware attacks that leverage Command and Control servers before they happen rather than depending on damage control measures. One highly recommended way of achieving this is by filtering DNS traffic which is commonly used for these attacks. Start protecting your network today with a [free DNSFilter account](https://app.dnsfilter.com/signup).
