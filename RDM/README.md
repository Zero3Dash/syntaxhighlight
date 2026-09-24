# Cisco IOS / IOS-XE / NX-OS Syntax Highlighting for Devolutions Remote Desktop Manager

`Cisco-IOS-XE-NXOS-SuperList-RDM.xml` is a single RDM syntax highlighting profile of 163 regex rules for Cisco IOS, IOS-XE and NX-OS terminal output. It was regression-tested against real output from two NX-OS switches, a Catalyst 9300 and an ISR4331.

## Contents

| Item                 | Value                                                 |
| -------------------- | ----------------------------------------------------- |
| Profile name         | `Cisco IOS / IOS-XE / NX-OS Super List`             |
| Profile ID           | `b1f5c0ae-3d42-4c19-9a2b-7e0f61d4c8a1`              |
| Rules                | 163                                                   |
| Regex flavour        | .NET                                                  |
| Case-sensitive rules | 61; the other 102 start with an explicit`(?i)`      |
| Tinted backgrounds   | 7 rules (listed under Palette); all others`#000000` |
| Encoding             | UTF-8, CRLF                                           |

## Install

1. Export the current syntax highlighting configuration as a backup.
2. In RDM, open the syntax highlighting configuration (Terminal settings for a session or connection type, or **File → Options → Types → Terminal**, depending on version).
3. Import `Cisco-IOS-XE-NXOS-SuperList-RDM.xml`. Re-importing replaces the profile with the same ID; other profiles are untouched.
4. Assign the profile to the relevant sessions, folder or default terminal type.
5. Delete the two throwaway probe profiles, `RDM Overlap Probe` and `RDM Engine Probe`, if they are still imported.

## Verified RDM behaviour

These were confirmed in RDM with the two probe profiles rather than assumed:

- **Overlap resolution:** every rule is matched against the raw line on its own. Where matches overlap, the earlier rule's colour wins for each character, and a later rule still colours the characters no earlier rule claimed.
- **Anchors:** `^` with `(?m)` anchors to the start of each terminal line, and `\s*$` matches at end of line.
- **Lookbehind:** variable-length lookbehind works; 13 rules depend on it.
- **Case:** `IsCaseSensitive` is honoured, and a rule with neither the flag nor `(?i)` matches case-insensitively.

Because the earliest rule wins, rules are ordered from most specific to most generic, and the two-digit prefix reflects that order.

## Rule groups

| Prefix | Group                   | Rules | Purpose                                                                                                                                                                                          |
| ------ | ----------------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 00     | Noise suppression       | 7     | Zero counters and rates,`hitcnt=0`, benign substrings, config values such as `timeout 60`, `95% idle`, `is not enabled`                                                                  |
| 01     | Typed commands          | 5     | Destructive commands (red background), config persistence, negations, interface and BGP neighbor shutdown; all anchored to the prompt                                                            |
| 02     | Structure               | 4     | Comments and remarks, descriptions, table headers, legend lines; kept neutral so their contents are not coloured as data                                                                         |
| 03     | Syslog                  | 11    | Adjacency up and down, link up, escalated events, success facilities, then severity 0-2, 3, 4-5, 6, 7                                                                                            |
| 04     | Counters and faults     | 14    | Non-zero error and drop counters, queue drops, load thresholds, reliability, CPU utilisation, environment faults, LOS/LOF, DOM alarm and warning markers, QoS drops                              |
| 05     | State                   | 34    | `ip int brief` and NX-OS status columns, IOS `show interfaces status`, BGP summary and table codes, OSPF, STP, port-channel members, redundancy, ping, traceroute, environment status, speed |
| 06     | Interfaces              | 7     | Long and short names, logical interfaces (including`TLS-VIF`), IOS-XR management, NX-OS slot ports, spaced names, console/vty                                                                  |
| 07     | Addressing              | 6     | Masks and wildcards, IPv4, FC WWN, IPv6, MAC, IS-IS NET                                                                                                                                          |
| 08     | Identifiers             | 7     | VLAN, VRF, AS numbers, RT/RD, BGP communities, VNI, bug IDs and CVEs                                                                                                                             |
| 09     | Routing protocols       | 9     | BGP, origin codes, OSPF, IS-IS, EIGRP, RIP, static/connected, metrics, route-code column                                                                                                         |
| 10     | MPLS and overlays       | 6     | MPLS/LDP, segment routing, VXLAN EVPN, DMVPN/NHRP/GRE, SD-WAN, OTV/LISP/FabricPath                                                                                                               |
| 11     | FHRP, L2, multicast     | 4     | HSRP/VRRP/GLBP, spanning tree, CDP/LLDP/LACP, PIM/IGMP/MSDP                                                                                                                                      |
| 12     | NX-OS platform          | 3     | vPC and dual-active, VDC/FEX/CoPP/features, StackWise/VSS/ISSU                                                                                                                                   |
| 13     | Security                | 9     | Plaintext keys, type 0/7 credentials, MD5 secrets, weak crypto and cleartext (not when negated with`no`), credentials, crypto, AAA, ACLs, ACL hit counts                                       |
| 14     | Services and QoS        | 11    | URLs, QoS, infrastructure services, transport protocols, NTP stratum 16, kiss codes, sys.peer, falseticker, sync state                                                                           |
| 15     | Software and hardware   | 10    | Versions, filesystems, image files, serial labels and long forms, Cisco serials, components, sensor readings, optics, optics PIDs                                                                |
| 16     | Time and rates          | 4     | Unsynced-clock marker, timestamps (ISO, syslog, IOS long form), uptime, rates and sizes                                                                                                          |
| 17     | Prompts and interaction | 8     | Config (amber background), enable, user exec, bootloader, hostname,`[confirm]`/`--More--`, progress, `% Invalid input`                                                                     |
| 18     | Generic vocabulary      | 4     | Down, failure, transitional and healthy words; last so every specific rule wins                                                                                                                  |

## Palette

Designed for a dark terminal background.

| Colour      | Meaning                                                                                                 |
| ----------- | ------------------------------------------------------------------------------------------------------- |
| `#FF5555` | Errors, down states, destructive commands, non-zero fault counters, plaintext or reversible credentials |
| `#F2C55C` | Warnings, transitional states, config persistence, config prompt, DOM warnings                          |
| `#5CE68A` | Up, established, success, forwarding, enable prompt                                                     |
| `#82AAFF` | Interfaces and VLANs                                                                                    |
| `#66D9EF` | IPv4 and IPv6 addresses                                                                                 |
| `#FFA657` | Routing protocols, VRF, AS, RT/RD, communities, ACLs, vPC, SD-WAN, BGP table status codes               |
| `#D19AFF` | MPLS, SR, VXLAN/EVPN, DMVPN, IS-IS, WWN                                                                 |
| `#FF79C6` | Multicast                                                                                               |
| `#FF92D0` | Credentials, crypto, VPN                                                                                |
| `#B4A0FF` | Spanning tree, L2 discovery, AAA, QoS, services, NX-OS platform                                         |
| `#9CDCFE` | Hardware, serials, optics, sensor readings                                                              |
| `#B5CEA8` | Software versions, images, filesystems, URLs                                                            |
| `#7FD1E8` | Informational syslog, metrics, rates, interactive prompts                                               |
| `#8A9199` | Neutral detail: MAC, masks, uptime, timestamps, descriptions, headers, legends                          |
| `#6E7681` | Suppressed noise and debug syslog                                                                       |
| `#5FD7AF` | EIGRP                                                                                                   |
| `#6FA96F` | Config comments, remarks and banners                                                                    |

Tinted backgrounds mark the highest-signal classes, so they do not rely on red versus green alone:

- **`#3A0000` (red):** destructive commands, severity 0-2 syslog, `ip int brief` protocol down, NX-OS protocol down, STP broken, ping with no replies.
- **`#3A2E00` (amber):** config mode prompt.

## Editing

Each rule is a `SyntaxHighlightingItem`:

```xml
<SyntaxHighlightingItem>
  <BackColorString>#000000</BackColorString>
  <BackgroundColor>Custom</BackgroundColor>
  <ForegroundColor>Custom</ForegroundColor>
  <ID>GUID</ID>
  <IsCaseSensitive>true</IsCaseSensitive>
  <IsCompleteWord>false</IsCompleteWord>
  <IsRegex>true</IsRegex>
  <Keyword>REGEX</Keyword>
  <Name>06 Interface short names</Name>
  <TextColorString>#82AAFF</TextColorString>
</SyntaxHighlightingItem>
```

Rules for editing:

- **Case mode:** every rule uses exactly one case mode. It either sets `IsCaseSensitive` to `true` or starts with `(?i)`.
- **Anchors:** use `\s*$` or `(?=\s*$)` for end anchors, so a trailing `\r` does not break the match.
- **Inline flags:** put inline flags at the start of the pattern.
- **XML:** escape `&`, `<` and `>` inside `<Keyword>`.
- **IDs:** each rule needs a unique ID. `build.py` derives new IDs from the rule name, so rebuilds keep IDs stable.
- **Word boundaries:** `IsCompleteWord` is false throughout because boundaries are written into the regexes.
- **Order:** a new specific rule must sit above any broader rule that could match the same characters.

The profile is generated by `build.py` (it reads the original list at the path in `SRC` and writes to `DST`). Edit `build.py` and rebuild rather than editing the XML by hand; the build asserts unique names and IDs, a single case mode per rule, and that every source rule is carried forward.

## Known trade-offs

These were reviewed and kept deliberately:

- **Syslog severity noise:** colours follow severity. `%ACLLOG-3-ACLLOG_FLOW_INTERVAL` and `%DAEMON-3-SYSTEM_MSG ... dcos_sshd` stay red, and routine `%SSH-5` session notices stay yellow.
- **ACL actions:** `permit` is green and `deny` is red everywhere, including intentional deny entries.
- **Non-redundant hardware:** on single-supervisor devices, `show redundancy` shows `Communications = Down  Reason: Failure` and `'DISABLED' state` in red. A line-based rule cannot tell simplex hardware from a failed peer.
- **Generic words:** the group 18 vocabulary matches single words anywhere, so banners and prose can be coloured (for example `UNAUTHORIZED` in a login banner). Extend group 00 rather than narrowing group 18.
- **`Active`:** treated as healthy (FHRP, chassis). In EIGRP topology output, a route stuck in active is not flagged.
- **SVI load:** NX-OS reports `txload 255/255` on some SVIs, so the 80 % load rule shows red there.
- **Weak-crypto keywords in context:** `deny ... eq telnet` and HSRP `authentication md5` are flagged as weak or cleartext.
- **Multi-line constructs:** anything wrapped across lines is only matched on the line where the pattern completes. Examples are wrapped IPv6 BGP neighbours (handled), banners after their first line, and wrapped descriptions.
- **DOM markers:** the `++ + - --` threshold markers are verified only against synthetic lines in Cisco's documented `show interfaces transceiver` format, because no device in the test estate reports DOM.
- **IOS-XR:** only the management interface name is covered; IOS-XR table layouts (for example the BGP `Spk` column) are not.
