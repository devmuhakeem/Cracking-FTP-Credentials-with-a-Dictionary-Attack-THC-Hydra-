# Cracking FTP Credentials with a Dictionary Attack (THC Hydra)

A hands-on lab demonstrating how weak, guessable passwords on an FTP service can be cracked with an automated dictionary attack, using Nmap for reconnaissance and THC Hydra for the attack itself, against a sandboxed lab target.

## Scenario
A target machine was running an FTP service. The exercise walked through identifying that service, confirming it required authentication, and then using a wordlist-based dictionary attack to find a working set of credentials and demonstrate real access.

## What I did

### 1. Confirmed the FTP service was live
- Ran a targeted Nmap scan against port 21 on the target: `nmap -p 21 [target IP]` — confirmed the port was open
- Connected directly with `ftp [target IP]` and was prompted for credentials, confirming the server required authentication (not anonymous access)
- Tried a couple of random guesses manually, which failed as expected, before moving to an automated approach

### 2. Ran a dictionary attack with THC Hydra
- Used a prepared wordlist pair, `Usernames.txt` and `Passwords.txt`
- Ran: `hydra -L [path]/Usernames.txt -P [path]/Passwords.txt ftp://[target IP]`
- Hydra systematically tried username/password combinations from the lists against the FTP service and returned a working, cracked credential pair

### 3. Verified and demonstrated the access
- Logged into the FTP server with the cracked credentials via `ftp [target IP]`
- Successfully authenticated and reached an interactive FTP session
- Created a new directory remotely with `mkdir Hacked`, then confirmed on the target machine itself that the directory had actually been created — turning a found credential into demonstrated write access

## Key takeaways
- A dictionary attack succeeds on the gap between what a password policy requires and what people actually choose — the wordlist's relevance matters more than the tool's sophistication
- Plain FTP compounds weak-credential risk by transmitting usernames and passwords in cleartext, a separate weakness layered on top of password strength
- Verifying impact (creating a file, not just logging in) is what turns a theoretical finding into a demonstrated, prioritizable vulnerability
- Defensive countermeasures are straightforward: strong/unique password enforcement, account lockout or rate-limiting after failed attempts, encrypted alternatives like SFTP/FTPS instead of plain FTP, and monitoring for bursts of failed authentication attempts

## Tools
Nmap, THC Hydra, FTP client, Parrot Security OS

---
*Completed in an isolated EC-Council training lab environment against a designated practice target.*
