---
# the default layout is 'page'
icon: fas fa-book-open
order: 5
layout: default
exclude-from-search: true
---

<h1><i class="fas fa-book-open"></i>&nbsp;Write-Ups</h1>

{% tabs platform %}
  {% tab platform#TryHackMe %}
    {% capture tab_content %}

## <img src="/tabs/images/tryhackme.png" width="25px"/> TryHackMe Write-ups

Below are the write-ups of the key details of each TryHackMe room that was done.

## Difficulty: <span class="badge rounded-pill bg-success" title="This is an Easy difficulty room."><i class="fa fa-bolt"></i>&nbsp;Easy</span>

| Name                                                         | Status                                                                                    | Summary                                                                                                                                                                                               |
| ------------------------------------------------------------ | ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.&nbsp;**[basicpentestingjt](thm/basicpentestingjt/index.html)** | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span>    | Enumerate Samba users (`enum4linux.pl`), `ssh2john` to crack private key.                                                                                                                             |
| 2. **[picklerick](thm/picklerick/index.html)**               | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span>    | Use `hydra` with `R1ckRul3s`, `sudo` to root the box.                                                                                                                                                 |
| 3. **rrootme**                                               | <span class="badge bg-info text-dark"><i class="fa fa-hourglass"></i>&nbsp;Pending</span> | TBD                                                                                                                                                                                                   |
| 4. **ohsint**                                                | <span class="badge bg-info text-dark"><i class="fa fa-hourglass"></i>&nbsp;Pending</span> | TBD                                                                                                                                                                                                   |
| 5. **[cowboyhacker](thm/cowboyhacker/index.html)**           | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span>    | Anonymous FTP with password list. Can `sudo` tar, and we use `--checkpoint`                                                                                                                           |
| 6. **[crackthehash](thm/crackthehash/index.html)**           | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span>    | Use `hash-identifier`, CrackStation, and `hashcat`.                                                                                                                                                   |
| 7. **[inclusion](thm/inclusion/index.html)**                 | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span>    | All Local File Inclusion (did not login)                                                                                                                                                              |
| 8. **[agentsudoctf](thm/agentsudoctf/index.html)**           | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span>    | Send custom `User-Agent`; `hydra` against FTP; `steghide`, `binwalk`, and `zip2john`; sudo with `-u#-1`                                                                                               |
| 9. **[overpass](thm/overpass/index.html)**                   | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span>    | Improper session handling; `ssh2john` to crack private key; `cron` job runs `buildscript.sh` that we edit. Metasploit option too.                                                                     |
| 10. **[lazyadmin](thm/lazyadmin/index.html)**                | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span>    | SweetRice CMS; Exposed `.sql` file; File Upload Bypass; Sudo `backup.pl` which calls our script.                                                                                                      |
| 11. **[ignite](thm/ignite/index.html)**                      | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span>    | Fuel CMS; ExploitDB Python script or File Upload Bypass; CVE with different paths; Shared MySQL password with root.                                                                                   |
| 12. **[startup](thm/startup/index.html)**                    | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span>    | Writable FTP for reverse shell; PCap in "incidents" has password; External cron runs `print.sh` that we control.                                                                                      |
| 13. **[tomghost](thm/tomghost/index.html)**                  | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span>    | Ghostcat exploits Apache Jserv Protocol; `john` and `gpg` cracking; We `sudo zip` and leverage `--unzip-command`.                                                                                     |
| 14. **[chillhack](thm/chillhack/index.html)**                | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span>    | A `/secret/` URL lets you run commands. `account.php` has DB credentials shared with user; send our SSH key to login; `steghide` and `fcrackzip` to PE. Docker filesystem breakout!                   |
| 15. **[bruteit](thm/bruteit/index.html)**                    | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span>    | We `hydra` brute force into website; `ssh2john` and `john` to login with SSH keys; Crack `passwd` and `shadow` with `john`.                                                                           |
| 16. **[fowsniff-ctf](thm/fowsniff-ctf/index.html)**          | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span>    | Simulated leaked creds on pastebin; Crack MD5 hashes; Credential Stuffing against SSH and POP3; Read emails, check for non-changed passwords; Command Injection for MOTD or ExploitDB for old Kernel. |
{: .table }
{: .table-dark }
{: .table-bordered }
{: .table-striped }
{: .table-hover }
{: .table-sm }
{: .table-responsive }

## DIfficulty: <span class="badge rounded-pill bg-warning text-dark" title="This is a Medium difficulty room."><i class="fa fa-wrench"></i>&nbsp;Medium</span>

| Name                                     | Status                                                                                 | Summary                                                                                                                                                  |
| ---------------------------------------- | -------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.&nbsp;**[mrrobot](thm/mrrobot/index.html)** | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span> | Use `hydra` to brute force passowrd for `elliot`; Edit Wordpress theme file to set up reverse shell; We can `su` because of crackable hash.              |
| 2. **[dogcat](thm/dogcat/index.html)**   | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span> | Use Local File Inclusion and HTTP header injection; Can sudo `env` so we get root, but in a container; Reverse shell container breakout via `backup.sh`. |
{: .table }
{: .table-dark }
{: .table-bordered }
{: .table-striped }
{: .table-hover }
{: .table-sm }
{: .table-responsive }

## Difficulty: <span class="badge rounded-pill bg-danger" title="This is a Hard difficulty room."><i class="fa fa-skull-crossbones"></i>&nbsp;Hard</span>

| Name                                           | Status                                                                                 | Summary                                                                                                                                       |
| ---------------------------------------------- | -------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.&nbsp;**[dailybugle](thm/dailybugle/index.html)** | <span class="badge bg-success"><i class="fa fa-check-circle"></i>&nbsp;Complete</span> | Joomla SQL Injection on old OS and kernel; `john` to decrypt one `bcrypt` password. PE by running `yum` plug-in since we can `yum` as `sudo`. |
{: .table }
{: .table-dark }
{: .table-bordered }
{: .table-striped }
{: .table-hover }
{: .table-sm }
{: .table-responsive }

    {% endcapture %}
    {{ tab_content | markdownify }}
{% endtab %}
  {% tab platform#HackTheBox %}
    {% capture tab_content %}

## <img src="/tabs/images/htb.jpg" width="35px"/> HackTheBox Write-ups

Below are the write-ups of the key details of each HackTheBox room that was done.

TBD

    {% endcapture %}
    {{ tab_content | markdownify }}    
  {% endtab %}

{% endtabs %}