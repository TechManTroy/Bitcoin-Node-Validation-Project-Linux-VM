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

<img width="569" height="439" alt="Screenshot 2025-11-11 222157" src="https://github.com/user-attachments/assets/06c75c0c-431b-4bf2-8bf2-eab773c7f0eb" />


- Image of created key

#

### Action: Command used to copy createed key
```
cat ~/.ssh/id_ed25519.pub
```

<img width="509" height="72" alt="Screenshot 2025-11-11 222454" src="https://github.com/user-attachments/assets/ca32fe5e-dee3-435f-81b8-f8c4e05d0b0d" />

- Image of command used to verify key was created so it could be copied

#

### Action: Adding Key to GitHub profile
<img width="958" height="276" alt="Screenshot 2025-11-10 160801" src="https://github.com/user-attachments/assets/cd459158-4bd6-4b33-8509-5f8f94718e91" />

- Image of key being added to Github profile


### Next action was allowing repository to accept SSH communicaton - Clone repo

```
$ git clone git@github.com:TechManTroy/Bitcoin-Node-Validation-Project-Linux-VM.git
```
<img width="559" height="246" alt="Screenshot 2025-11-11 222725" src="https://github.com/user-attachments/assets/faa165bc-ed24-4505-a128-c0281bbd3192" />

- Image of repository being cloned

#



## 2. Global Repository Exclusion Rules (.gitignore_global)
A global exclusion file was implemented on the Windows 11 host to maintain a clean repository and protect against accidental exposure of sensitive local files across all Git projects.


### Action: Configuration Command: The following command set the global rule:

```
git config --global core.excludesfile C:/Users/yourusername/Git/.gitignore_global
```

<img width="561" height="62" alt="Screenshot 2025-11-11 223219" src="https://github.com/user-attachments/assets/4db82a2a-5b39-46c4-93bf-bcc0a9b675ce" />

- Image of command used to set gitignore.global

#



### Action: Command used to display global gitignore was created
```
git config --global core.excludesfile
```

<img width="556" height="68" alt="Screenshot 2025-11-11 223449" src="https://github.com/user-attachments/assets/3ccfb0f5-7253-466d-9074-9f0c9ebdb5ea" />

- Image of command used to display gitignore was created

#


## 3. Project-Specific Exclusion Rules (.gitignore)
The project's local .gitignore file was created and committed to prevent the exposure of runtime configuration secrets, which is a common security failure point.

### Key Security Exclusions Tracked:

* bitcoin.conf (Contains RPC credentials).

* electrum-personal-server/config.ini (Contains RPC credentials and the wallet's XPUB).

* .bitcoin/ directory (Contains the entire blockchain database).
  <img width="653" height="489" alt="Screenshot 2025-11-10 164232" src="https://github.com/user-attachments/assets/01c4e396-a227-4654-afd2-e3669762c8dc" />

- Image of Key Security Exclusions Tracked
#
