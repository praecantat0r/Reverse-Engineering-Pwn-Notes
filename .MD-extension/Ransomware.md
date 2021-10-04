# Ransomware
## The History
- Ransomware is a type of malware that prevents or limits the user from accessing their system, either by encryption or by locking the system's screen.
- The first ever ransomware was the AIDS TROJAN written by Joseph Popp in 1989. First iterations of extortionate ransomware began in Russia during the years 2005/2006.
- Then came the police ransomware and then, in 2013, the CryptoLockers.

> ![[Pasted image 20210923162655.png]]

- After all of this we come to the age of big-game hunting and double extortion. So they target big names and the threaten to leak their shit.
- Some of the big groups include: **BlackMatter, REvil, RagnarLocker, Lockbit 2.0, Darkside, Babuk**....

> ![[Pasted image 20210923164124.png]]
-                       notable ransomware attacks of 2020

## Ransomware 101
- All modern Ransomware usually
> 1. Generates a key
> 2. Tries to exfiltrate the key through sending it somewhere.
> 3. Stops online services
> 4. Looks for drives and then files of interest by iterating through a list.
> 5. Encrypts by walking down the file system and encrypting every file of interest with a specified encryption method.
> 6. After all of this it can rename the files to hamper file identification efforts, or use a fixed file extension
> 7. And after all of that all that is needed to do is to drop a Note with a ransom note and a bitcoin wallet address, and optionally clear the Logs.

## Autopsy
- We are going to dissect the code from the Babuk leak. It is written in C++. Attacks both Windows and Linux platforms.
 >![[Pasted image 20210924102558.png]]
 
- There are a few header files and some interesting c++ files.
- Starting of with entry.cpp we begin with a BABUK_KEYS, BABUK_SESSION and BABUK_FILEMETA structures. Moving on we have an _encrypt_file function:
 >![[Pasted image 20210924103144.png]]
 All files that are encrypted get a .babyk extension and have "choung dong looks like hot dog!!”" written at the end of them.
 Elliptic-curve Diffie–Hellman (ECDH) scheme is used for file key encryption.
 And Curve25519 is used as the elliptic curve.

- Function for finding files:
> ![[Pasted image 20210924104915.png]]
> It searchs for files not much else to be observed here but it uses a blacklist to filter files it doesnt want to hit.
> Some of the notable file names include: Windows, Tor browser, Internet Explorer, Google, Appdata... 
> > ![[Pasted image 20211003213922.png]]
- And for finding paths:
> ![[Pasted image 20210924105341.png]]
- After that we have a _processDrive function which uses the LPCWSTR (Long Pointer to Constant Wide String) *driveLetters* from **another.cpp**
> ![[Pasted image 20210924110107.png]]
- in the *entry* function we call _stop_services, _stop_processes, _remove_shadows from **another.cpp** which uses a list of processes and services that should be stopped, and remove shadows uses vssadmin.exe to delete shadow copies:
 `ShellExecuteW(0, L"open", L"cmd.exe", L"/c vssadmin.exe delete shadows /all /quiet", 0, SW_HIDE);`
> ![[Pasted image 20210924110906.png]]
> ![[Pasted image 20210924111244.png]]
 
- Also there are SHA-256, SHA-512, HC-128m and SOSEMANUK implementations in sha256.cpp, sha512.cpp, hc-128.cpp and sosemanuk.cpp.
- Mutex to check for running copies: DoYouWantToHaveSexWithCuongDong (reference to the researcher Chuong Dong, who did an analysis of the previous babuk ransomware versions)
> ![[Pasted image 20211003213758.png]]

- And that is all i can disect with my limited Malware Analysis knowledge




