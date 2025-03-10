# ssh

## odpowiednik ssh-copy-id na widnowsa (powerShell)
```powershell
ssh-keygen -t rsa
type $env:USERPROFILE\.ssh\id_rsa.pub | ssh {IP-ADDRESS-OR-FQDN} "cat >> .ssh/authorized_keys"
```
