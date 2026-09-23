# Cisco IOS / IOS-XE / NX-OS Syntax Highlighting for Devolutions Remote Desktop Manager

`Cisco-IOS-XE-NXOS-SuperList-RDM.xml` is a single RDM syntax highlighting profile containing 102 regex rules for Cisco IOS, IOS-XE and NX-OS terminal output. It is a merge and rewrite of four SecureCRT keyword lists into RDM's `SyntaxHighlightingProfileList` schema.

## Contents

| Item | Value |
| --- | --- |
| Profile name | `Cisco IOS / IOS-XE / NX-OS Super List` |
| Profile ID | `b1f5c0ae-3d42-4c19-9a2b-7e0f61d4c8a1` |
| Rules | 102 |
| Regex flavour | .NET |
| Case-sensitive rules | 17 (rest rely on inline `(?i)`) |
| Background | `#000000` on all rules except `01 Destructive commands` (`#3A0000`) |
| Encoding | UTF-8, CRLF |

## Install

1. Back up the existing profile: export the current syntax highlighting configuration before importing.
2. In RDM, open the syntax highlighting configuration (Terminal settings for a session or connection type, or **File → Options → Types → Terminal** depending on version).
3. Import `Cisco-IOS-XE-NXOS-SuperList-RDM.xml`.
4. Assign the profile `Cisco IOS / IOS-XE / NX-OS Super List` to the relevant sessions, folder, or the default terminal type.
5. Verify against live output from `show interfaces`, `show ip bgp summary`, `show logging`, `show inventory`.

The file declares one profile only, so importing it does not modify or remove other profiles. Re-importing replaces the profile with the same ID rather than duplicating it.

## Rule ordering

Rules are named with a two-digit prefix and stored in ascending order, on the assumption that RDM evaluates top-down and the first match wins. Noise suppression is therefore first and broad topology vocabulary is last.

| Prefix | Group | Rules | Purpose |
| --- | --- | --- | --- |
| 00 | Noise suppression | 6 | Zero counters, zero rates, `hitcnt=0`, benign substrings such as `no shutdown`, `downstream`, `fallback` |
| 01 | Destructive and config-changing commands | 3 | `write erase`, `reload`, `format`, `commit replace`, `copy run start`, `no router …` |
| 02 | Syslog by severity | 7 | `%FACILITY-n-MNEMONIC` split across severity 0–2, 3, 4–5, 6, 7, plus success facilities and adjacency loss |
| 03 | Non-zero counters and faults | 8 | Leading and trailing error counters, input queue drops, `rxload`/`txload`, reliability, 90 %+ utilisation, PSU/fan/thermal |
| 04 | Interface and session state | 7 | Administratively down, down states, failure vocabulary, transitional, up/healthy, bundled members, negotiated speed |
| 05 | Interfaces | 7 | Long names, short names, logical interfaces, IOS-XR `MgmtEth0/RP0/CPU0/0`, NX-OS `eth1/1`, spaced names, console/vty |
| 06 | Addressing | 6 | IPv4 with mask or port, masks and wildcards, IPv6 with prefix length, MAC (both notations), FC WWN, IS-IS NET |
| 07 | Identifiers | 6 | VLAN, VRF, AS numbers, route targets and RDs, VNI, Cisco bug IDs and CVEs |
| 08 | Routing protocols | 9 | BGP, origin codes, OSPF, IS-IS, EIGRP, RIP, static/connected, route metrics, route-code column |
| 09 | MPLS and overlays | 6 | MPLS/LDP, segment routing, VXLAN EVPN, DMVPN/NHRP/GRE, SD-WAN, OTV/LISP/FabricPath |
| 10 | FHRP, L2, multicast | 4 | HSRP/VRRP/GLBP, spanning tree, CDP/LLDP/UDLD/LACP, PIM/IGMP/MSDP |
| 11 | NX-OS platform | 3 | vPC and consistency checks, VDC/FEX/CoPP/features, StackWise/VSS/ISSU |
| 12 | Security | 6 | Credentials, crypto and VPN, weak crypto and cleartext, AAA, ACLs and filters, non-zero ACL hits |
| 13 | Services and QoS | 4 | QoS, infrastructure services, transport protocols, NTP sync |
| 14 | Software and hardware | 7 | Versions, image files, filesystems, serials and PIDs, components, optics, sensor readings |
| 15 | Time and rates | 3 | Uptime, timestamps, rates and sizes |
| 16 | Prompts and interaction | 10 | Config/enable/user prompts, hostname, `[confirm]` and `--More--`, progress, comments, descriptions, `% Invalid input`, table headers |

If your RDM build applies the last match instead of the first, reverse the order of `SyntaxHighlightingItem` elements.

## Palette

Designed for a dark terminal background.

| Colour | Meaning |
| --- | --- |
| `#FF5555` | Errors, down states, destructive commands, non-zero fault counters |
| `#F2C55C` | Warnings, transitional states, config persistence, config prompt |
| `#5CE68A` | Up, established, success, negotiated speed, enable prompt |
| `#66D9EF` | IPv4 and IPv6 addresses |
| `#FFD75F` | Interfaces and VLANs |
| `#FFA657` | Routing protocols, VRF, AS, RT/RD, ACLs, vPC, SD-WAN |
| `#D19AFF` | MPLS, SR, VXLAN/EVPN, DMVPN, IS-IS, WWN |
| `#FF79C6` | Multicast |
| `#FF92D0` | Credentials, crypto, VPN |
| `#B4A0FF` | Spanning tree, L2 discovery, AAA, QoS, services, NX-OS platform |
| `#9CDCFE` | Hardware, optics, sensor readings |
| `#B5CEA8` | Software versions, images, filesystems |
| `#7FD1E8` | Informational syslog, metrics, rates, interactive prompts |
| `#8A9199` | Neutral detail: MAC, masks, uptime, timestamps, descriptions, headers |
| `#6E7681` | Suppressed noise and debug syslog |
| `#5FD7AF` | EIGRP |
| `#6FA96F` | Config comments and banners |

## Editing

Each rule is a `SyntaxHighlightingItem` with this element order:

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
  <Name>05 Interface short names</Name>
  <TextColorString>#FFD75F</TextColorString>
</SyntaxHighlightingItem>
```

Rules:

- `IsCaseSensitive` is omitted when absent; it defaults to false. Rules that must respect case set it to `true` and carry no `(?i)`.
- Case-insensitive rules carry an explicit leading `(?i)`, which is safe whichever default the engine applies.
- `(?m)` is required for any pattern anchored with `^` or `$`, since output is matched per line by rule rather than per buffer.
- Inline flags must appear at the start of the pattern. `(?i)` or `(?m)` mid-pattern is valid .NET but breaks portability with the generator and other engines.
- `&`, `<` and `>` must be XML-escaped inside `<Keyword>`.
- A new rule needs a fresh GUID in `ID`; duplicate IDs collide on import.
- `IsCompleteWord` is false throughout because word boundaries are expressed in the regexes; Cisco output uses `/ . : -` as separators, which most word-boundary implementations treat as delimiters.

## Known trade-offs

- Generic vocabulary rules (`04 Failure vocabulary`, `04 Up and healthy states`) match single words anywhere on a line, so they colour prose in banners and MOTDs. The `00` group absorbs the common false positives; extend it rather than narrowing the vocabulary rules.
- `Active` is treated as healthy (FHRP and chassis semantics). In EIGRP topology output `Active` indicates a route stuck in active state and is not flagged.
- `04 Down states` matches `(s)`, `(D)`, `(SD)` and `(RD)` for suspended or down port-channel members; these strings also appear in unrelated output.
- `07 Route targets and RDs` includes a bare `\d+:\d+` alternative, which can match arbitrary colon-separated numbers.
- `12 Weak crypto and cleartext` flags `md5`, `sha1`, `3des` and `telnet` unconditionally, including inside documentation or capability lists.
- Multi-line constructs are not supported. Anything spanning a line break (an OSPF database block, a wrapped `description`) is matched only on the line where the pattern completes.

## Sources merged

- VanDyke `Cisco Words - DkBg.ini` (the list in the supplied `TEST-export.xml` reference).
- TakeshiTogo `SecureCRT-Cisco-Highlighting`, `Lab Highlights.ini`.
- janjrukiyavivek `securecrt-network-highlights`, `Network Engineer Ultimate.ini`.
- feralpacket `securecrt-keyword-highlighting`, `feralpacket2025_phrases.ini`.

SecureCRT semantics that do not carry over: the `[*]Section` comment pseudo-entries (replaced by the `Name` field), the `0000001f` style bitmasks for bold/reverse video, the gray catch-all default-colour rule, and the SecureCRT word-delimiter behaviour that let those lists omit `\b`.

## Validation

The profile parses as XML, every `Keyword` compiles as a regex, and every `Name` is unique. Element names and ordering match the reference export, so the file imports through the same code path.
