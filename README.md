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
