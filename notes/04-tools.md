# Chapter 04 — Cybersecurity Tools

**Written by Anji | Hyderabad, India**
**Level — Beginner | Reading time — 20 minutes**

---

Every profession has its tools. A carpenter has a hammer and a saw. A doctor has a stethoscope and a scalpel. A cybersecurity professional has a set of tools that they reach for to understand systems, find weaknesses, and demonstrate vulnerabilities.

Every tool in this chapter is completely free. No paid versions. No trials. No credit card required. Most of them come pre-installed if you are using Kali Linux.

Before we start, one important rule — use these tools only on your own systems, your own home network, or on practice platforms like TryHackMe and HackTheBox that are specifically designed for this kind of testing. Using them on systems you do not own, or do not have written permission to test, is illegal in India under the Information Technology Act.

---

## Kali Linux — The Starting Point

**Download:** https://www.kali.org/get-kali/

Kali Linux is not a tool itself — it is an entire operating system built specifically for cybersecurity. Think of it as a toolbox that already comes completely filled. When you install Kali, you immediately have over 600 security tools ready to use, including every other tool mentioned in this chapter.

It is made by a company called Offensive Security, who also created the OSCP certification — widely considered the gold standard for penetration testers.

The easiest way to start is to install VirtualBox (free from virtualbox.org) on your Windows or Mac, then download the Kali Linux virtual machine image. You import it into VirtualBox, and Kali runs inside a window on your existing computer. Your normal system is completely untouched. The default login is the username `kali` and the password `kali`.

Once you are inside, open a terminal and run `sudo apt update && sudo apt upgrade` to make sure everything is current. Then you are ready to learn.

---

## Nmap — Seeing What Is Running on a Network

**Download:** https://nmap.org/download.html
**Pre-installed on Kali:** Yes
**Practice on TryHackMe:** https://tryhackme.com/room/furthernmap

Nmap, short for Network Mapper, is the first tool almost every penetration tester uses on any engagement. Its job is to find out what is on a network — what devices exist, what ports are open on those devices, and what software is running.

Before an attacker can exploit anything, they need to understand their target. Nmap gives them that picture. Defenders use it for exactly the same reason — to see their own network from an attacker's perspective and identify anything unexpected.

When you run Nmap against an IP address, it sends carefully crafted packets to thousands of ports and analyses the responses to determine which ones are open, which are closed, and which are being filtered by a firewall. It can then probe the open ports further to identify what software is listening, what version it is, and sometimes even what operating system the machine is running.

For a beginner, start simple. Run `nmap 192.168.1.1` against your own home router. You will see which ports are open on it. Then try `nmap -sV 192.168.1.1` to see what software is running on each open port. The `-sV` flag stands for service version detection. Running `nmap 192.168.1.0/24` scans your entire home network and shows you every device connected to it, which is always interesting — you might discover devices you forgot were connected.

Do not worry about memorising every option. Start with basic scans, understand what the output is telling you, and gradually explore more advanced options as your understanding grows.

---

## Wireshark — Reading Network Traffic

**Download:** https://www.wireshark.org/download.html
**Pre-installed on Kali:** Yes
**Practice on TryHackMe:** https://tryhackme.com/room/wireshark101

If Nmap shows you the surface of a network, Wireshark shows you what is flowing through it.

Wireshark is a packet analyser. It captures all the network traffic passing through your network interface in real time and lets you read each packet individually. You can see exactly what your computer is sending and receiving, decoded into human-readable form.

The educational value of Wireshark is enormous. Open it, start a capture, then browse a few websites, send a message, make a DNS lookup. Then stop the capture and scroll through the packets. You will see DNS queries going out asking for IP addresses, TCP handshakes establishing connections, HTTP requests going to web servers, and responses coming back. The abstract concepts from Chapter 02 become concrete and visible.

Wireshark also demonstrates in the most direct possible way why HTTPS matters. If you capture traffic to an HTTP website (one without the padlock), you can read the content directly — including any form submissions or login credentials. If you capture traffic to an HTTPS website, all you see is encrypted data that looks like meaningless noise. This is not a theoretical difference. It is visible right in front of you.

For beginners, the most useful thing to learn first is filtering. Wireshark captures thousands of packets per minute, which can be overwhelming. Typing `http` in the filter bar shows only web traffic. Typing `dns` shows only DNS lookups. Typing `ip.addr == 192.168.1.5` shows only traffic involving one specific device. Learning a few filters makes the data manageable.

---

## Burp Suite — Testing Web Applications

**Download:** https://portswigger.net/burp/communitydownload
**Free web security labs:** https://portswigger.net/web-security
**Pre-installed on Kali:** Yes (Community Edition)

Burp Suite is the standard tool for testing the security of web applications. It works as a proxy — it sits between your browser and the website you are testing, intercepting every request and response so you can examine and modify them.

When you browse a website normally, requests go from your browser directly to the server. With Burp Suite running as a proxy, those requests pass through Burp first. You can see every detail — the headers, the parameters, the cookies, the body of the request. You can change anything you want, then forward the modified request to the server and watch what happens.

This is how web security testers find vulnerabilities. They look at how the application handles their requests, change values in unexpected ways, and watch whether the application handles the unexpected input safely or breaks in interesting ways.

The Community Edition (free) is excellent for learning. The paid Professional version adds automated scanning, but you do not need it as a beginner. Alongside Burp Suite, the company behind it — PortSwigger — offers the Web Security Academy, which is the most comprehensive free web security training available anywhere. It includes hands-on labs for every topic, from SQL injection to cross-site scripting to business logic vulnerabilities.

Start by setting Burp up as a proxy in Firefox, browsing a website, and just reading the intercepted requests. Before trying to find vulnerabilities in anything, spend time simply understanding what you are looking at.

---

## Metasploit — Using Known Vulnerabilities

**Download:** https://www.metasploit.com/download
**Pre-installed on Kali:** Yes
**Safe practice target:** https://sourceforge.net/projects/metasploitable/
**Practice on TryHackMe:** https://tryhackme.com/room/metasploitintro

Software vulnerabilities are discovered constantly. When researchers find a vulnerability in a piece of software, they often write an exploit — code that takes advantage of that vulnerability to gain unauthorised access. Metasploit is a framework that collects thousands of these exploits and makes them easy to use in a controlled, professional way.

Think of it as a library. When a vulnerability is found in a piece of software — say, a particular version of a web server — someone writes an exploit module for Metasploit. That module knows exactly how to talk to the vulnerable software in a way that causes it to misbehave and give the attacker access. Instead of writing that exploit from scratch, a penetration tester can simply find the right module, point it at a target, and run it.

Metasploit is used both offensively (to demonstrate that a vulnerability is actually exploitable) and defensively (to verify that a patch actually fixed what it claimed to fix).

For practice, download Metasploitable 2 — a virtual machine that was deliberately built full of vulnerabilities for exactly this purpose. Run it in VirtualBox alongside your Kali VM. Now you have a completely legal, completely safe target to practice on. You can run Metasploit from Kali, point it at Metasploitable, and learn how exploitation actually works without touching any real system.

---

## Gobuster — Finding Hidden Parts of Websites

**Download:** https://github.com/OJ/gobuster/releases
**Pre-installed on Kali:** Yes
**Practice on TryHackMe:** https://tryhackme.com/room/contentdiscovery

When developers build websites, they sometimes create pages or folders that are never linked from anywhere — admin panels, backup files, configuration pages, old testing directories. These exist on the server but there is no link pointing to them, so regular visitors never find them.

Gobuster finds these hidden resources by trying thousands of common names rapidly. It takes a list of words — common directory names, common file names — and systematically tries each one against the target website. If a request returns a valid response instead of a "not found" error, that resource exists, even though it is not publicly linked.

The wordlists it uses come from collections of real-world names that have been found on websites over the years. Kali Linux includes several wordlists in the `/usr/share/wordlists` folder, including the famous rockyou.txt and various directory lists.

Finding an exposed admin panel, a backup file containing source code, or a configuration file containing database credentials through Gobuster is one of the most common ways web applications get compromised in real penetration tests.

---

## Nikto — Quickly Scanning a Website for Problems

**Download:** https://github.com/sullo/nikto
**Pre-installed on Kali:** Yes

Nikto is an automated web scanner. You point it at a website and it rapidly checks for thousands of known issues — outdated server software, default files that should have been removed, headers that should be set but are not, and known vulnerabilities associated with the software versions it detects.

It is not a deep, thorough scanner — Burp Suite does that job better. Nikto is fast and noisy (it makes no attempt to hide its scanning activity), and it gives you a quick overview of the obvious problems with a web server in just a few minutes.

For beginners, Nikto is a good early exercise because it gives you results quickly without requiring you to understand every detail of what it is doing. You can see real vulnerabilities identified against a real web server, which makes the concepts feel concrete.

---

## John the Ripper — Understanding Password Security

**Download:** https://www.openwall.com/john/
**Pre-installed on Kali:** Yes
**Practice on TryHackMe:** https://tryhackme.com/room/johntheripper0

When an attacker gains access to a database, they often find passwords stored as hashes — a scrambled representation of the password that cannot be directly reversed. John the Ripper takes those hashes and tries to recover the original passwords.

The way it works reveals exactly why password strength matters so much.

John works by taking words from a list, hashing each one, and comparing the result to the hash it is trying to crack. If the hashes match, the original word was the password. The rockyou.txt wordlist, which comes with Kali and contains about 14 million real passwords from a historical data breach, can crack an astonishing number of passwords in minutes because people routinely use exactly those passwords.

John can also apply rules — taking words from the list and automatically generating variations like capitalising the first letter, adding numbers at the end, substituting letters for symbols. "Password" becomes "Password1", "p@ssword", "P4ssword", and hundreds of other variations. Many people think these substitutions make passwords secure. John's rules show exactly why they do not.

Practising with John teaches you more about password security than any theoretical explanation. When you crack a hash in three seconds using a wordlist, you understand immediately why "password123" is not acceptable. When you see a hash resist cracking even after running through millions of variations, you understand what actually makes a password strong.

---

## Hydra — Testing Login Forms

**Download:** https://github.com/vanhauser-thc/thc-hydra
**Pre-installed on Kali:** Yes
**Practice on TryHackMe:** https://tryhackme.com/room/hydra

While John cracks stored password hashes offline, Hydra attacks login forms directly over the network. It takes a list of usernames and passwords and tries each combination against an actual login page — SSH, FTP, web forms, and many other services.

Security professionals use Hydra to demonstrate that a system's passwords are too weak and that an account lockout policy is needed. Without lockout, there is nothing stopping an attacker from trying thousands of combinations automatically.

The protection against Hydra and tools like it is simple but must be implemented. Accounts should lock after a small number of failed attempts. Two-factor authentication should be required. Strong, unique passwords should be enforced. When all three are in place, brute-forcing becomes practically impossible.

---

## SQLmap — Finding Database Vulnerabilities

**Download:** https://sqlmap.org/
**Pre-installed on Kali:** Yes
**Free practice labs:** https://portswigger.net/web-security/sql-injection

SQL injection is one of the oldest and still most common web vulnerabilities. It happens when a web application takes user input and includes it directly in a database query without properly handling it. An attacker can then insert SQL commands as input, manipulating the query to extract data they should not be able to access.

SQLmap automates the process of detecting and exploiting SQL injection vulnerabilities. You give it a URL, it analyses the parameters, tests them in various ways, and tells you whether any are vulnerable. If they are, it can extract the database structure, dump tables, and retrieve data.

Understanding SQLmap is valuable not just for exploitation but for defence. When you see exactly how an injection attack works in practice — watching SQLmap retrieve usernames and password hashes from a vulnerable test application — the importance of parameterised queries and input validation becomes completely concrete.

Practice SQLmap using the Web Security Academy labs from PortSwigger, which are free and specifically designed to demonstrate these vulnerabilities safely.

---

## Netcat — Sending Raw Data Across a Network

**Pre-installed on Kali and most Linux systems:** Yes

Netcat is a small tool that can open raw network connections and send or receive data through them. It is extraordinarily simple, but that simplicity makes it useful in an enormous variety of situations, which is why people call it the Swiss Army knife of networking.

You can use it to test whether a port is open on a remote machine. You can use it to transfer files between machines. You can use it to create a simple chat session between two computers. And in penetration testing, it is commonly used to create reverse shells — a technique where a compromised machine reaches back out to the attacker's machine and gives them a command prompt.

Understanding how to use Netcat is one of the practical skills that separates someone who has read about hacking from someone who can actually do it. It is worth spending time becoming comfortable with it.

---

## Where to Practice All of This Safely

Knowing about tools is only the beginning. You need to actually use them on real systems to develop skill. These platforms are specifically designed to give you legal targets.

**TryHackMe** at https://tryhackme.com is the best starting point for beginners. It has guided learning paths that walk you through using each tool step by step, with hints available when you get stuck. The free tier gives you access to a large number of rooms. Many security professionals recommend starting with the Pre-Security path and then the Jr Penetration Tester path.

**HackTheBox Academy** at https://academy.hackthebox.com provides structured modules with hands-on labs. It is slightly more challenging than TryHackMe and less hand-holding, which makes it excellent once you have some foundations. The free tier covers substantial content.

**PortSwigger Web Security Academy** at https://portswigger.net/web-security is the best free resource specifically for web application security. Every topic comes with a hands-on lab using a real vulnerable application. It is comprehensive, professional, and completely free.

**Metasploitable 2** is a deliberately vulnerable Linux virtual machine. Download it, run it alongside your Kali VM, and practice Nmap, Metasploit, Nikto, and other tools against it in your own environment with no internet required.

---

## The Order to Learn These In

Do not try to learn every tool at the same time. That leads to knowing a little about everything and being effective with nothing.

Start with Nmap. Spend a week just scanning your home network, reading the output, understanding what the results mean. When you understand what you are seeing, move to Wireshark. Spend a week capturing and reading your own traffic. Then set up Burp Suite and work through the first few modules of the PortSwigger Web Security Academy.

By the time you have spent a month with those three tools, you will have a level of practical understanding that most people who have only read about cybersecurity will not have. From there, add one new tool at a time, always tying it to actual practice rather than just reading documentation.

---

## What Comes Next

You now have a clear picture of the tools available to you and how they fit together. The chapters that follow cover how these tools are used in practice — ethical hacking methodology, web security vulnerabilities, and eventually your first CTF challenges.

**[Continue to Chapter 05 — Ethical Hacking Methodology →](./05-ethical-hacking.md)**

---

*Written by Anji | Hyderabad, India | 2025*
*If this helped you, please star the repository.*
