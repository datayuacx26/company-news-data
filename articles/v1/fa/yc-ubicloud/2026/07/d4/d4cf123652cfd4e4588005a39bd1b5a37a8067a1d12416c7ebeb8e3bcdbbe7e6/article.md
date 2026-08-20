---
schema_version: "1.0.0"
document_id: "d4cf123652cfd4e4588005a39bd1b5a37a8067a1d12416c7ebeb8e3bcdbbe7e6"
company_key: "yc-ubicloud"
company: "Ubicloud"
source_id: "yc-ubicloud-news-import-c10303752e5c"
canonical_url: "https://www.ubicloud.com/blog/hardware-touch-stronger-ssh"
published_at: null
first_seen_at: "2026-07-24T05:09:51.579811+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:4c761d7e868ec5ec032b8fadb68b5679b09d0d0651b04205c363b1d8ed25a041"
---

# Hardware Touch, Stronger SSH

## Hardware Touch, Stronger SSH


October 9, 2025 · 6 min read


Daniel Farina


Founder / CTO


Every time I log into a server or push code to GitHub, a small USB key on my laptop flashes. I have to touch it to continue. That extra tap blocks a class of attacks that’s far more common now than twenty years ago: malware stealing or abusing your SSH keys. Setting this up takes little more effort than distributing new SSH keys every couple of years. In this post I’ll cover why you should take that extra time and explain how to use the technology you can adopt to achieve this.
‍
Covered below are:


- Why malware on developer laptops is a real threat today
- How touch-verified SSH stops it
- Practical implementation techniques


### Why you should care


Twenty years ago, my rough education about malware attacks suggested they:


- Targeted Windows primarily
- Affected non-technical staff more often
- Would only be encountered on the seedier corners of the Internet
- Were sophisticated only with the involvement of a state actor


Today, especially after cryptocurrency and ransomware made attacks more profitable, well-funded attackers focus on professionals and infrastructure.\[1\] Twenty years ago, these attack vectors would have seemed theoretical. Only the most valuable targets would have faced them:


- Sham recruiters attack recruits with malicious interview payloads\[2\]
- Sham requests for code review target reviewers, such as contractors\[3\]
- Supply chain attacks on security programs that intended to improve security.\[4\]
- Supply chain attacks on trusted utilities\[5\]
- Typosquatting in package repositories\[6\]
- VSCode extensions\[7\]
- Upstream projects that are gradually transferred to bad actors\[8\]


The balance has shifted, but operating systems have not caught up with usable isolation for professional work. The exception is ambitious projects like Qubes.\[9\] We need other measures. Touch-verified SSH is an inexpensive, low-impact intervention that helps.


### How touch verification helps


If you have used SSH with public and private keys to push to GitHub or log into servers, you will recognize this experience:
After rebooting your machine, you try to push to a Git repository. You are prompted for a key password, or perhaps you configured the system to unlock your key at login. Either way, subsequent Git operations require no password.


This relies on the ssh-agent protocol, integrated with your operating system: Keychain on macOS, GNOME Keyring on Linux, or the standard OpenSSH implementation on Windows. This interaction dates back to 1995, when SSH was introduced as "freeware," before OpenSSH existed.


The model has two critical weaknesses:


- **Key exfiltration** : Malware can capture your key password, steal your private key, and decrypt it elsewhere.
- **Silent abuse** : Even without your passphrase, the agent allows SSH key use without your knowledge, logging into servers or pushing code invisibly.


Touch-verified SSH solves both problems:


- **Hardware isolation** : The private key lives in a separate device with no protocol for reading it. Exfiltration requires special equipment and physical access, like side-channel attacks on electromagnetic signatures.keycloning
- **Physical confirmation** : You must physically touch the key, creating an out-of-band channel that prevents silent key use.


### Implementation techniques


#### MacOS with built-in hardware


Apple laptops after 2018 excel at having hardware to make touch verification work smoothly, at no additional hardware cost. Touch ID uses a "Secure Enclave" co-processor, an isolated secondary computer with restricted communication. That's the fingerprint reader in the upper-right corner by the keyboard. Unfortunately, macOS does not include SSH agent support for Touch ID/Secure Enclave. You will need[Secretive](https://github.com/maxgoedjen/secretive) . In my experience, and that of the Ubicloud team, Secretive is extremely reliable. Beyond fingerprint verification, it can store non-touch-verified keys in the secure enclave for development work. These keys require no touch but cannot be copied, providing a security bonus for less-critical tasks.


Secretive guides you through creating enclave-resident keys. Just distribute the public key to servers needing authentication, like any SSH public key. Each key use triggers a notification about the cryptographic operation, making unexpected activity from compromised machines more visible. The software is impressive. The author deserves recognition.


#### FIDO2 security keys for other platforms


Since I do not use macOS daily, I rely on FIDO2 security keys: USB devices costing $10-$60. You can use FIDO2 security keys on macOS too, though Apple's OpenSSH builds lack FIDO2 support. Installing via brew install openssh provides FIDO2-enabled OpenSSH. Why Apple did not include this functionality is unclear—it could be an oversight. Many vendors make FIDO2 security keys. Yubico's YubiKeys are well-known and offer "nano"-scale USB-C products essential for laptops without USB-A ports. I prefer nano products to reduce damage risk; they are too small to snag on anything.


At Ubicloud, we provide two keys per staff member: one primary, one backup. Some prefer larger backup keys with nano primaries. These keys also work for WebAuthn ("passkey") authentication on websites. As with SSH, hardware touch verification is far more secure than software-only variants in password managers or browsers.


After buying and plugging in a security key, run:


```text
ssh-keygen -t ed25519-sk


```


This prints something like:


```text
Generating public/  private   ed25519-sk key pair.
```


Follow the usual SSH keypair steps, including setting an encryption passphrase.


Distribute the public key to servers or GitHub as normal.The key works like any SSH key, except you must touch the hardware for authentication during git push or ssh. You might wonder how security improves when it generates a "private key" file with passphrase encryption. The answer: calling it a private key is practical mis-naming.\[11\] It is actually a key handle. This handle combines with a private key held in the hardware. Together, they derive the real private key that matches the public key seen in id_ed25519_sk.pub.


Thus, you need both the "private key" file and matching hardware to authenticate.


An optional but highly recommended step is to **disable OTP mode on YubiKeys** . You'll notice this if accidentally touching the key makes it type random strings like:


```text
ccccccjlkgjlevtdernkbbnrrvhcvdbljgchbgbdbvgk
```


This creates chaos when typed into Gmail, GitHub, or applications with hotkeys. Unless you specifically need OTP, disable it. Install ykman via Homebrew, apt, or[other means](https://docs.yubico.com/software/yubikey/tools/ykman/Install_ykman.html) , then run:


```text
ykman config mode FIDO
```


Remove and reinsert the YubiKey. It remembers this setting permanently.


Also retrieve your YubiKey's serial number. Though printed on the device (even nano ones), it's hard to read:


```text
ykman   list   --serials
```


You can uninstall ykman afterward.


#### FIDO2 security keys for other platforms


Since these keys cannot be cloned or copied, you will likely maintain multiple keys for redundancy, convenience, or multiple computers. Everyone occasionally replaces hardware. With macOS and Secretive, this happens every few years since keys cannot transfer between devices. This raises questions about which keys to invalidate and how to link each key with its required hardware. Given the low number of keys per person and infrequent changes, a simple approach works well: maintain a Git repository with an authorized_keys format file:


```text
[email protected]
AAAAGnNrLXNzaC1lZDI1NTE5QG9wZW5zc2guY29tAAAAILChQ1wpGdpxlrs448d98x6/F1xKmXOSfJ5pzJ4DExLOA
AAABHNzaDo= daniel+  24345989  @ubicloud.com  [email protected]
AAAAGnNrLXNzaC1lZDI1NTE5QG9wZW5zc2guY29tAAAAIKLkgjxaqNssfWx35m1sALAvTFg+Wi1rBOOixXv56u7RA
AAABHNzaDo= daniel+  26335085  @ubicloud.com ecdsa-sha2-nistp256
AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBFkGB1JeNyhEhK+  6  u7PsGVFcsNuTawnm/G8YbxpZpW3bm8aodOAkNbEJjyzRZmlN9Yp/bwW0HX8eXLFWUHH9460=  [email protected]
```


Group each person's keys together. The file works directly as ~/.ssh/authorized_keys.


You need a complete list of every active key. This lets you match key digests from OpenSSH's LogLevel VERBOSE output with specific people and hardware.\[12\] The output looks like this:


```text
Aug   29     04  :  11  :  36   vmb0zabr sshd  [  1265  ]  : Accepted publickey   for   ubi from   2601  :  645  :c57e:  6  a80:  3  aa6:  4732  :  81  fd:ba3c port   42526   ssh2: ED25519-SK SHA256:gfLueg2RoqE7w6g7wcQeNcyOE8TbqhkOgXJetXe7HCQ
```


A Git repository gives you timestamped commits and pull requests that show who added which keys and when. Branch protection and reviews add approval records, making auditing and compliance work the same way as code.


The comment section (last column) identifies the person and hardware. I use email addresses for precise identification, combined with Gmail's "plus addressing" to include hardware serial numbers. I prefer serial numbers: they are often written on the surface of the device by the manufacturer. This makes identification possible even for broken or powered-down hardware.


Both YubiKeys and Apple computers have serial numbers written onto the surface of the device. They are also available via software. For YubiKeys, use ykman list --serials. For Apple computers, use ioreg -l | grep IOPlatformSerialNumber.


#### Using touch verification judiciously


I reserve touch verification for production systems and maintain some touch-unverified keys too.


This reduces mistakes: touching the security key reminds me to increase caution. Using it for all work weakens this effect. I encourage staff to submit touch-unverified keys in separate authorized_keys files alongside production keys. These serve as development substitutes and ease staff collaboration.


I consider GitHub pushes important enough for touch verification. Malicious code pushes without my consent could be catastrophic. I also have commit access to widely-used non-Ubicloud repositories. Given touch verification's ease, I have an ethical obligation to reduce compromise risk through my laptop.


Watch for unexpected touch requests: they may indicate machine compromise. Be alert if you must touch the key twice for one intended action repeatedly, as one of those touches might assist malicious software.


Next up


[Quick Start - Build your own cloud](https://www.ubicloud.com/docs/quick-start/build-your-own-cloud)


### Conclusion


The world today is far more subtle and malicious in targeting software engineers. There are only a few low-cost measures that can make a real difference. This is one of them.


### Resources


1. WithSecure writes on the economic structure and increased prevalence of attack infrastructure, financed by ransomware:"the root of all evil" .
2. [ReversingLabs reports on the VMConnect campaign](https://www.reversinglabs.com/blog/fake-recruiter-coding-tests-target-devs-with-malicious-python-packages) where attackers pose as recruiters and send malicious Python packages disguised as coding tests to target developers.
3. In which a sham client[tries to attack a developer evaluating accepting a contract](https://www.codementor.io/@chihebnabil/caught-malicious-code-during-a-code-review-on-the-platfrom-be-careful-out-there-2lxy9e05zu)
4. Wikipedia article on the 2020 SolarWinds supply chain attack that compromised thousands of organizations including U.S. government agencies through malicious updates to the Orion software.[https://en.wikipedia.org/wiki/2020_United_States_federal_government_data_breach](https://en.wikipedia.org/wiki/2020_United_States_federal_government_data_breach)
5. CrowdStrike analysis of how the popular CCleaner utility was compromised with a backdoor, affecting millions of users in a supply chain attack.[https://www.crowdstrike.com/en-us/blog/protecting-software-supply-chain-deep-insights-ccleaner-backdoor/](https://www.crowdstrike.com/en-us/blog/protecting-software-supply-chain-deep-insights-ccleaner-backdoor/)
6. Typosquatting in package repositories:[PyPI, npm, RubyGems](https://pmc.ncbi.nlm.nih.gov/articles/PMC7338168/) ,[go modules](https://ssojet.com/news/go-module-mirror-backdoor-3-year-supply-chain-attack-exposed) ,[rust cargo](https://www.sentinelone.com/labs/cratedepression-rust-supply-chain-attack-infects-cloud-ci-pipelines-with-go-malware/) If there are dependencies, there are attacks.
7. [A quantitative survey](https://www.koi.security/blog/2-6-exposing-malicious-extensions-shocking-statistics-from-the-vs-code-marketplace) of malicious extensions by Koi Security
8. Wikipedia article on the XZ Utils backdoor, where a maintainer gradually introduced malicious code into a widely-used compression library over several years.[https://en.wikipedia.org/wiki/XZ_Utils_backdoor](https://en.wikipedia.org/wiki/XZ_Utils_backdoor)
9. [Qubes OS](https://www.qubes-os.org/) , a security-focused operating system that uses virtual machines with a lot of work in the window manager, network, and file system access to try to make the whole thing usable.
10. (https://arstechnica.com/security/2024/09/yubikeys-are-vulnerable-to-cloning-attacks-thanks-to-newly-discovered-side-channel/) on a sophisticated side-channel vulnerability in YubiKey 5 series that allows cloning through electromagnetic emanation analysis, but it requires expensive equipment and extensive physical access (including melting off the plastic cover)
11. Formally, the "private key" randomly generated by ssh-keygen when making a sk type key is a "key handle" (FIDO1) or "credentialID" (FIDO2). This data gets sent to the FIDO2 device for each cryptographic operation and combines with factory-sealed private material to derive the private key matching the public key file. This allows a single hardware key to serve unlimited identities by generating multiple key handles and offloading their storage to less-constrained devices like laptops, keeping the hardware stateless except for its factory-sealed material.
12. I have been referring to[Mozilla Infosec's OpenSSH guidance](https://infosec.mozilla.org/guidelines/openssh) for many years to configure OpenSSH. It slowly changes with the times, and it's a good reference for the security-conscious.
