## key takeaways:
- domain controllers don't have its own "Local Users and Groups" setting. Domain member computers can have their own "Local users and Groups". However DCs can have built-in local Administrator group that if any non "Domain Admins" group user have, they can still use ADUC (Active Directory Users and Computers) with full rights.

> "Computer Management" -> "System tools" -> "Local Users and Groups"

- Domain Users cannot by-default login to Domain Controller, unless explicitly set.

## LLMNR poisoning
#### what is LLMNR?
Link-Local Multicast Name Resolution (LLMNR) is used to identify and resolve hosts and hostnames when DNS fails to do so.
![[Pasted image 20260906175010.png]]
- previously, **NBT-NS (NetBIOS over TCP/IP Name Service)** is being used.
- key flaw is that the services utilise user's username and NTLMv2 password hash when appropriately responded to.
- to capture username and NTLMv2 hash, we will use "Responder" tool. this is the first thing that should be running in the background in the pentest engagement.
#### mitigations
- Disable LLMNR, NBT-NS
- if cannot disable LLMNR / NBT-NS, then
	- require Network Access Control
	- require strong user passwords.

![[Pasted image 20260907141857.png]]

## NTLM relay attack
NTLM (New-Technology LAN Manager) 
#### what is NTLM?
A Microsoft authentication protocol used to verify identity when accessing windows system and network resources.

It uses a challenge–response process:
1. The server sends a random challenge.
2. The client combines it with a password-derived hash.
3. The client sends back the response.
4. The server verifies it without receiving the plaintext password.

The NTLM challenge-response process involves:
- NT hash: `MD4(UTF-16LE(password))`. This is the password-derived value stored or obtained by the authentication system.
- NTLMv1 response: Uses the NT hash as a DES key source to encrypt the server’s 8-byte challenge, producing a 24-byte response.
- NTLMv2 response: Uses the NT hash as the key for an HMAC-MD5 operation involving the username, domain, server challenge, client challenge, timestamp, and other data.
```
password
   ↓ MD4
NT hash
   ↓
NTLMv1 or NTLMv2 challenge-response calculation
   ↓
response sent to the server
```

#### what is NTLM relay attack?
- NTLM relay attack involves capturing live authentication exchange and forwarding it to the victim to authenticate as that user without cracking the password-derived hash.
- NTLM relay attack is different from pass-the-hash attack. pth reuses a stolen NT hash, while relay attack forwards live authentication exchange to another service.
- `impacket-ntlmrelayx` tool automates NTLM relay.
	- it listens for inbound NTLM authentication on protocols like SMB, HTTP, LDAP etc.
	- forwards the captured exchange to SMB, LDAP supported services.
- there are SMB, LDAP, HTTP, MSSQL relay attacks as well but they are all sub-part of NTLM relay attack where the NTLM authenticates to the destination services like SMB, LDAP, HTTP, MSSQL respectively.

**Requirements**
- victim must authenticate to attacker-controlled listener.
- the target must accept NTLM.
- the target service must lack effective relay protections.

**Defenses**
- Prefer kerberos and reduce or disable NTLM.
- Require SMB and LDAP signing.

