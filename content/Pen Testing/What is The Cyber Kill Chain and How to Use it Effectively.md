---
dg-publish: true
---

The cyber kill chain is a series of steps that trace [stages of a cyberattack](https://www.varonis.com/blog/anatomy-of-a-breach-sony/?hsLang=en) from the early reconnaissance stages to the exfiltration of data. The kill chain helps us understand and combat ransomware, security breaches, and advanced persistent attacks (APTs).

[Lockheed Martin](http://www.lockheedmartin.com/content/dam/lockheed/data/corporate/documents/LM-White-Paper-Intel-Driven-Defense.pdf) derived the kill chain framework from a military model – originally established to identify, prepare to attack, engage, and destroy the target. Since its inception, the kill chain has evolved to better anticipate and recognize insider threats, social engineering, advanced ransomware and innovative attacks.

### Get a free Data Risk Assessment

[![cyber kill chain definition](https://info.varonis.com/hs-fs/hubfs/Imported_Blog_Media/cyber-kill-chain@2x.png?width=3000&height=1089&name=cyber-kill-chain@2x.png)](https://info.varonis.com/hubfs/Imported_Blog_Media/cyber-kill-chain@2x.png?hsLang=en)

## How the Cyber Kill Chain Works

There are several core stages in the cyber kill chain. They range from reconnaissance (often the first stage in a malware attack) to lateral movement (moving laterally throughout the network to get access to more data) to data exfiltration (getting the data out).  All of your common attack vectors – whether phishing or brute force or the latest strain of malware – trigger activity on the cyber kill chain.

[![cyber kill chain phases](https://info.varonis.com/hs-fs/hubfs/Imported_Blog_Media/cyber-kill-chain-phases-2@2x.png?width=3001&height=1724&name=cyber-kill-chain-phases-2@2x.png)](https://info.varonis.com/hubfs/Imported_Blog_Media/cyber-kill-chain-phases-2@2x.png?hsLang=en)

Each stage is related to a certain type of activity in a cyber attack, regardless of whether it’s an internal or external attack:

-   **Reconnaissance**  
    The observation stage: attackers typically assess the situation from the outside-in, in order to identify both targets and tactics for the attack.
-   **Intrusion**  
    Based on what the attackers discovered in the reconnaissance phase, they’re able to get into your systems: often leveraging malware or security vulnerabilities.
-   **Exploitation**  
    The act of exploiting vulnerabilities, and delivering malicious code onto the system, in order to get a better foothold.
-   **Privilege Escalation**  
    Attackers often need more privileges on a system to get access to more data and permissions: for this, they need to escalate their privileges often to an Admin. ^5506c2
-   **Lateral Movement**  
    Once they’re in the system, attackers can move laterally to other systems and accounts in order to gain more leverage: whether that’s higher permissions, more data, or greater access to systems.
-   **Obfuscation / Anti-forensics**  
    In order to successfully pull off a cyberattack, attackers need to cover their tracks, and in this stage they often lay false trails, compromise data, and clear logs to confuse and/or slow down any forensics team.
-   **Denial of Service**  
    Disruption of normal access for users and systems, in order to stop the attack from being monitored, tracked, or blocked
-   **Exfiltration**  
    The extraction stage: getting data out of the compromised system.

Below, we’ll explore each phase of the cyber kill chain in more detail.

## 8 Phases of The Cyber Kill Chain

[![cyber kill chain in depth phases](https://info.varonis.com/hs-fs/hubfs/Imported_Blog_Media/KC-bgadd.png?width=1500&height=1000&name=KC-bgadd.png)](https://info.varonis.com/hubfs/Imported_Blog_Media/KC-bgadd.png?hsLang=en)

Each phase of the kill chain is an opportunity to stop a cyberattack in progress: with the right tools to detect and recognize the behavior of each stage, you’re able to better defend against a systems or data breach.

### Reconnaissance

In every heist, you’ve got to scope the joint first. Same principle applies in a cyber-heist: it’s the preliminary step of an attack, the information gathering mission. During reconnaissance, an attacker is seeking information that might reveal vulnerabilities and weak points in the system. Firewalls, intrusion prevention systems, perimeter security – these days, even social media accounts – get ID’d and investigated. Reconnaissance tools [scan corporate networks to search for points of entry](https://www.varonis.com/blog/port-scanning-techniques/?hsLang=en) and vulnerabilities to be exploited.

### Intrusion

Once you’ve got the intel, it’s time to break in. Intrusion is when the attack becomes active: attackers can send [malware](https://www.varonis.com/blog/cryptolocker/?hsLang=en) – including ransomware, spyware, and adware – to the system to gain entry. This is the delivery phase: it could be delivered by [phishing email](https://www.varonis.com/blog/whaling-attack/?hsLang=en), it might be a compromised website or that really great coffee shop down the street with free, hacker-prone wifi. Intrusion is the point of entry for an attack, getting the attackers inside.

### Exploitation

You’re inside the door, and the perimeter is breached. The exploitation stage of the attack…well, exploits the system, for lack of a better term. Attackers can now get into the system and install additional tools, modify security certificates and create new script files for nefarious purposes.

### Privilege Escalation

What’s the point of getting in the building, if you’re stuck in the lobby? Attackers use privilege escalation to get elevated access to resources. Privilege escalation techniques often include [brute force attacks](https://www.varonis.com/blog/brute-force-attack/?hsLang=en), preying on password vulnerabilities, and exploiting [zero day vulnerabilities](https://www.varonis.com/blog/zero-day-vulnerability/?hsLang=en). They’ll modify GPO security settings, configuration files, change permissions, and try to extract credentials. ^4214be

### Lateral Movement

You’ve got the run of the place, but you still need to find the vault. Attackers will move from system to system, in a lateral movement, to gain more access and find more assets. It’s also an advanced [data discovery](https://www.varonis.com/products/data-classification-engine?hsLang=en) mission, where attackers seek out critical data and sensitive information, admin access and email servers – often using the same resources as IT and leveraging built-in [tools like PowerShell](https://www.varonis.com/blog/windows-powershell-tutorials/?hsLang=en) – and position themselves to do the most damage. ^666162

### Obfuscation (anti-forensics)

Put the security cameras on a loop and show an empty elevator so nobody sees what’s happening behind the scenes. Cyber-attackers do the same thing: conceal their presence and mask activity to avoid detection and thwart the inevitable investigation. This might mean wiping files and metadata, overwriting data with false timestamps (timestomping) and misleading information, or modifying critical information so that it looks like the data was never touched.

### Denial of Service

Jam the phone lines and shut down the power grid. Here’s where the attackers target the network and data infrastructure, so that the legitimate users can’t get what they need. The [denial of service (DoS) attack](https://www.varonis.com/blog/what-is-a-ddos-attack/?hsLang=en) disrupts and suspends access, and could crash systems and flood services.

### Exfiltration

Always have an exit strategy. The attackers get the data: they’ll copy, transfer, or move sensitive data to a controlled location, where they do with the data what they will. Ransom it, sell it on ebay, send it to wikileaks. It can take days to get all of the data out, but once it’s out, it’s in their control.

## The Takeaway

Different security techniques bring forward different approaches to the cyber kill chain – everyone from [Gartner](http://blogs.gartner.com/ramon-krikken/2014/08/08/introducing-gartners-cyber-attack-chain-model/) to [Lockheed Martin](http://cyber.lockheedmartin.com/solutions/cyber-kill-chain) defines the stages slightly differently. Alternative models of the cyber kill chain combine several of the above steps into a C&C stage (command and control, or C2) and others into an ‘Actions on Objective’ stage. Some combine lateral movement and privilege escalation into an exploration stage; others combine intrusion and exploitation into a ‘point of entry’ stage.

It’s a model often criticized for focusing on perimeter security and limited to malware prevention. When combined with [advanced analytics and predictive modeling](https://www.varonis.com/products/data-security-platform/?hsLang=en), however, the cyber kill chain becomes critical to data security.

With the above breakdown, the kill chain is structured to reveal the active state of a data breach. Each stage of the kill chain requires specific instrumentation to detect cyber attacks, and Varonis has out-of-the-box threat models to [detect those attacks at every stage of the kill chain](https://www.varonis.com/products/datalert/?hsLang=en).

Varonis monitors attacks at the entry, exit, and everywhere in between. By [monitoring outside activity – like VPN, DNS, and Proxy](https://www.varonis.com/products/edge/?hsLang=en), Varonis helps guard the primary ways to get in and out of an organization.  By monitoring file activity and user behavior, Varonis can detect attack activity on every stage of the kill chain – from [kerberos attacks](https://www.varonis.com/blog/kerberos-attack-silver-ticket/?hsLang=en) to [malware behavior](https://www.varonis.com/blog/malware-protection-defending-data-with-varonis-security-analytics/?hsLang=en).

Want to see it in action? See how Varonis addresses each stage of the kill chain in a [1:1 demo](https://info.varonis.com/demo?hsLang=en) – and learn how you can prevent and stop ongoing attacks before the damage is done.

### What you should do now

Below are three ways we can help you begin your journey to reducing data risk at your company:

1.  [Schedule a demo session with us](https://info.varonis.com/en/demo-request?hsLang=en), where we can show you around, answer your questions, and help you see if Varonis is right for you.
2.  [Download our free report](https://info.varonis.com/en/great-saas-data-exposure-report?hsLang=en) and learn the risks associated with SaaS data exposure.
3.  Share this blog post with someone you know who'd enjoy reading it. Share it with them via [email,](mailto:?subject=What%20is%20The%20Cyber%20Kill%20Chain%20and%20How%20to%20Use%20it%20Effectively&body=I%20think%20you%20might%20find%20this%20interesting%3A%20https://www.varonis.com/blog/cyber-kill-chain) [LinkedIn,](https://www.linkedin.com/shareArticle?mini=true&url=https://www.varonis.com/blog/cyber-kill-chain&title=What%20is%20The%20Cyber%20Kill%20Chain%20and%20How%20to%20Use%20it%20Effectively) [Twitter,](https://twitter.com/intent/tweet?text=What%20is%20The%20Cyber%20Kill%20Chain%20and%20How%20to%20Use%20it%20Effectively&url=https://www.varonis.com/blog/cyber-kill-chain) [Reddit,](http://www.reddit.com/submit?url=https://www.varonis.com/blog/cyber-kill-chain&title=What%20is%20The%20Cyber%20Kill%20Chain%20and%20How%20to%20Use%20it%20Effectively) or [Facebook.](https://www.facebook.com/sharer/sharer.php?u=https://www.varonis.com/blog/cyber-kill-chain)

×

![Michael Buckbee](https://info.varonis.com/hubfs/Varonis_June2021/Images/michael-buckbee-200x200-150x150.jpg)

##### Michael Buckbee

Michael has worked as a sysadmin and software developer for Silicon Valley startups, the US Navy, and everything in between.