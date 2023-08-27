---
# the default layout is 'page'
icon: fas fa-note-sticky
order: 4
---

## Commands

### Command: `nmap`

```bash
nmap -sCV <IP>
```

### Command: `gobuster`

```bash
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://<IP>
```

### Command: `nikto`

```bash
nikto -h <IP>
```

### Command: `hydra`

#### OPTION 1. SSH with fixed username and password wordlist

```bash
hydra -l jdoe -P /usr/share/wordlists/rockyou.txt \
    ssh://<IP>
```

#### OPTION 2: SSH with username wordlist and fixed password

```bash
hydra -L ./usernames.txt -p admin \
    ssh://<IP>
```

#### OPTION 3: Webform with fixed username and password wordlist

```bash
hydra -l jdoe -P /usr/share/wordlists/rockyou.txt \
    <IP> http-post-form \
    "/login.php:username=^USER^&password=^PASS^&sub=Login:F=Invalid username or password."
```

#### OPTION 4: Webform with username wordlist and fixed password

```bash
hydra -L ./usernames.txt -p admin \
    <IP> http-post-form \
    "/login.php:username=^USER^&password=^PASS^&sub=Login:F=Invalid username or password."
```

### Command: `searchsploit`

```bash
searchsploit <keyword>
```

In the output, the location of those files are in: `/usr/share/exploitdb/exploits/`

### Command `nc` - NetCat for listening/receiving

```bash
nc -lvnp 9000
```

## Concepts

### Concept: Upgrading a fragile shell with Python `pty`

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

### Concept: Seeing what commands current user can use as `sudo`

```bash
sudo -l
```