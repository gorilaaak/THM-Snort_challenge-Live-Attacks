# THM Snort Challenge: Live Attacks

Put our Snort-ing skills into practice during live attacks.

## Introduction

The room invites you to a challenge where you will investigate a series of traffic data and stop malicious activity under two different scenarios. Let's start working with Snort to analyse live and captured traffic. 

![Untitled](THM%20Snort%20Challenge%20Live%20Attacks%20c6c50b4f685b406c93d3537708db46b1/Untitled.png)

## Scenario 1: Brute Force

J&Y Enterprise is one of the top coffee retails in the world. They are known as tech-coffee shops and serve millions of coffee lover tech geeks and IT specialists every day. They are famous for specific coffee recipes for the IT community and unique names for these products. Their top five recipe names are: **WannaWhite, ZeroSleep, MacDown, BerryKeep and CryptoY.**

J&Y's latest recipe, "Shot4J", attracted great attention at the global coffee festival. J&Y officials promised that the product will hit the stores in the coming months.

The super-secret of this recipe is hidden in a digital safe. Attackers are after this recipe, and J&Y enterprises are having difficulties protecting their digital assets.

During boring SoC shift nearly coming to an end, we have been engaged by J.A.V.A - our virtual assistant.

**[+] J.A.V.A.**

**Welcome, sir. I am sorry for the interruption. It is an emergency. Somebody is knocking on the door!**

**[+] J.A.V.A.**

**We have a brute-force attack, sir.**

**[+] J.A.V.A.**

**Sir, you need to observe the traffic with Snort and identify the anomaly first. Then you can create a rule to stop the brute-force attack. GOOD LUCK!**

Looks like we are under brute force attack - lets begin the investigation. First of all we need to fire up Snort into sniffer mode to see traffic in real time to perform very basic recon.

![Untitled](THM%20Snort%20Challenge%20Live%20Attacks%20c6c50b4f685b406c93d3537708db46b1/Untitled%201.png)

![Untitled](THM%20Snort%20Challenge%20Live%20Attacks%20c6c50b4f685b406c93d3537708db46b1/Untitled%202.png)

As we can mainly two ports are targeted - 22 and 80. Lets write TCP rule since both services utilize TCP to narrow down the search.

![Untitled](THM%20Snort%20Challenge%20Live%20Attacks%20c6c50b4f685b406c93d3537708db46b1/Untitled%203.png)

![Untitled](THM%20Snort%20Challenge%20Live%20Attacks%20c6c50b4f685b406c93d3537708db46b1/Untitled%204.png)

Looks better, now lets tweak our rule to target only port 22 which is SSH - SSH brute-forcing is very popular way to obtain initial access by iterating over wordlist of popular username and passwords with applications such **Hydra.** Lets rerun Snort with our SSH rule.

![Untitled](THM%20Snort%20Challenge%20Live%20Attacks%20c6c50b4f685b406c93d3537708db46b1/Untitled%205.png)

![Untitled](THM%20Snort%20Challenge%20Live%20Attacks%20c6c50b4f685b406c93d3537708db46b1/Untitled%206.png)

As we can see traffic is originating from one IP address and targeting one address on our side -  lets tweak our rule to narrow down the specific IP’s and rerun.

![Untitled](THM%20Snort%20Challenge%20Live%20Attacks%20c6c50b4f685b406c93d3537708db46b1/Untitled%207.png)

![Untitled](THM%20Snort%20Challenge%20Live%20Attacks%20c6c50b4f685b406c93d3537708db46b1/Untitled%208.png)

Looks good - now we will attempt to stop the attack.

**Write an IPS rule and run Snort in IPS mode to stop the brute-force attack. Once you stop the attack properly, you will have the flag on the desktop! What is the flag?**

Once we have all details required to stop the attack, we will write and IDS rule and run the Snort in full mode. We will use **reject** option because it will also terminate the TCP session by setting **RST** flag.

![Untitled](THM%20Snort%20Challenge%20Live%20Attacks%20c6c50b4f685b406c93d3537708db46b1/Untitled%209.png)

After less than a minute flag appeared on our desktop.

**Answer:** THM{81b7fef657f8aaa6e4e200d616738254}

## Scenario 2: Reverse-Shell

**[+] You**

**Thanks team. J.A.V.A. can you do a quick scan for me? We haven't investigated the outbound traffic yet.**

**[+] J.A.V.A.**

**Yes, sir. Outbound traffic investigation has begun.**

**[+] THE NARRATOR**

**The outbound traffic? Why?**

**[+] YOU**

**We have stopped some inbound access attempts, so we didn't let the bad guys get in. How about the bad guys who are already inside? Also, no need to mention the insider risks, huh? The dwell time is still around 1-3 months, and I am quite new here, so it is worth checking the outgoing traffic as well.**

**[+] J.A.V.A.**

**Sir, persistent outbound traffic is detected. Possibly a reverse shell...**

**[+] YOU**

**You got it!**

**[+] J.A.V.A.**

**Sir, you need to observe the traffic with Snort and identify the anomaly first. Then you can create a rule to stop the reverse shell. GOOD LUCK!**

Looks like we stopped the attack by setting inbound rule but we need to observe also the outbound traffic. Lets fire up Snort in sniffer mode and look for un-usual port numbers.

![Untitled](THM%20Snort%20Challenge%20Live%20Attacks%20c6c50b4f685b406c93d3537708db46b1/Untitled%2010.png)

![Untitled](THM%20Snort%20Challenge%20Live%20Attacks%20c6c50b4f685b406c93d3537708db46b1/Untitled%2011.png)

Port 4444 is often used by **Metasploit** framework as listener port, also a lots of malware types is using this port and since is also used by popular services such Kerberos is often overlooked by Firewalls. Lets again narrow our search by setting the rule to look for bi-directional (both ways) communication on port 4444 and rerun Snort.

![Untitled](THM%20Snort%20Challenge%20Live%20Attacks%20c6c50b4f685b406c93d3537708db46b1/Untitled%2012.png)

![Untitled](THM%20Snort%20Challenge%20Live%20Attacks%20c6c50b4f685b406c93d3537708db46b1/Untitled%2013.png)

Okay we detected the traffic and since is non-standard port such SSH we do not need to narrow the reject rule to specific IP’s - we will just drop every attempt on these ports and rerun Snort this time in full mode.

![Untitled](THM%20Snort%20Challenge%20Live%20Attacks%20c6c50b4f685b406c93d3537708db46b1/Untitled%2014.png)

After less than minute our flag appeared on the screen.

**Answer:** THM{0ead8c494861079b1b74ec2380d2cd24}

And that is it, i hope you liked the challenge - stay safe.