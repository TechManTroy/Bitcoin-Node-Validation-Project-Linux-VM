# 🛡️ Git & Workflow Security Configuration
This section documents the configuration of the Git version control system, focusing on secure remote access (SSH) and maintaining a clean, secure repository via exclusion files.

## 1. Repository Authentication Protocol (SSH)
The HTTPS protocol was bypassed in favor of SSH to secure the connection between the Windows Host Machine and the GitHub repository. SSH is generally preferred for automation and security as it relies on key pairs rather than passwords, which are susceptible to credential exposure.

* Protocol Implemented: SSH (Secure Shell)

* Key Type Used: ED25519 (Modern, secure key algorithm)

* Security Feature: A strong passphrase was used to encrypt the private key (id_ed25519), preventing unauthorized use even if the key file is compromised.

* Verification: The remote host's key fingerprint was manually verified upon the first connection to prevent potential Man-in-the-Middle attacks.




### Action: Command used to create secure key
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

<img width="569" height="459" alt="Screenshot 2025-11-10 155714" src="https://github.com/user-attachments/assets/943a0695-8d3e-43ef-9576-634d87609356" />


### Action: Command used to copy createed key
```
cat ~/.ssh/id_ed25519.pub
```
<img width="511" height="72" alt="Screenshot 2025-11-10 160204" src="https://github.com/user-attachments/assets/2b20443e-445e-478b-aeb3-48d99b9c8d00" />


### Action: Adding Key to GitHub profile
<img width="958" height="276" alt="Screenshot 2025-11-10 160801" src="https://github.com/user-attachments/assets/cd459158-4bd6-4b33-8509-5f8f94718e91" />


### Next action was allowing repository to accept SSH communicaton - Clone repo

```
$ git clone git@github.com:TechManTroy/Bitcoin-Node-Validation-Project-Linux-VM.git
```
<img width="559" height="249" alt="Screenshot 2025-11-10 160629" src="https://github.com/user-attachments/assets/f5b19555-6a07-4599-b294-f8e9e6bb22cd" />


## 2. Global Repository Exclusion Rules (.gitignore_global)
A global exclusion file was implemented on the Windows 11 host to maintain a clean repository and protect against accidental exposure of sensitive local files across all Git projects.


### Action: Configuration Command: The following command set the global rule:

```
git config --global core.excludesfile C:/Users/troya/Git/.gitignore_global
```

<img width="574" height="87" alt="Screenshot 2025-11-10 153131" src="https://github.com/user-attachments/assets/6234550c-ecb8-4483-9389-ce8179a20584" />


### Action: Command used to display global gitignore was created
```
git config --global core.excludesfile
```
<img width="565" height="362" alt="Screenshot 2025-11-10 153604" src="https://github.com/user-attachments/assets/cb843910-bed9-4f94-ab13-14abcaf7028a" />

## 3. Project-Specific Exclusion Rules (.gitignore)
The project's local .gitignore file was created and committed to prevent the exposure of runtime configuration secrets, which is a common security failure point.

### Key Security Exclusions Tracked:

* bitcoin.conf (Contains RPC credentials).

* electrum-personal-server/config.ini (Contains RPC credentials and the wallet's XPUB).

* .bitcoin/ directory (Contains the entire blockchain database).
  <img width="653" height="489" alt="Screenshot 2025-11-10 164232" src="https://github.com/user-attachments/assets/01c4e396-a227-4654-afd2-e3669762c8dc" />

