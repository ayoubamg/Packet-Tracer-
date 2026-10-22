# Unternehmensnetzwerk mit VLANs und IT-Sicherheit

**Cisco Packet Tracer Portfolio-Projekt | Fachinformatiker für Systemintegration**

## Ziel
Aufbau eines kleinen Unternehmensnetzwerks mit VLAN-Segmentierung, Router-on-a-Stick, DHCP, DNS/Webserver und ACL-Regeln zum Schutz interner Netze.

> **Wichtig:** Dieses Paket enthält Konfigurationen, Dokumentation und eine Topologie-Grafik. Eine echte Packet-Tracer-Datei (`.pkt`) und echte Packet-Tracer-Screenshots müssen in Cisco Packet Tracer erstellt werden; sie werden hier nicht vorgetäuscht. Baue die Topologie nach, teste die Befehle und speichere anschließend die `.pkt`-Datei und eigene Screenshots in den vorgesehenen Ordnern.

## Geräte und Verkabelung
- Router: Cisco 2911 (oder kompatibel)
- Switch: Cisco 2960
- Server: DNS + HTTP
- PCs: Administration, IT, Mitarbeiter, Gast
- Router `G0/0` ↔ Switch `F0/1` (802.1Q-Trunk)
- Server ↔ Switch `F0/2` (Access VLAN 20)
- PC-ADMIN ↔ Switch `F0/3` (Access VLAN 10)
- PC-IT ↔ Switch `F0/4` (Access VLAN 20)
- PC-MITARBEITER ↔ Switch `F0/5` (Access VLAN 30)
- PC-GAST ↔ Switch `F0/6` (Access VLAN 40)

## VLAN- und IP-Adressplan
| VLAN | Abteilung | Subnetz | Gateway | DHCP-Bereich |
|---|---|---|---|---|
| 10 | Administration | 192.168.10.0/24 | 192.168.10.1 | 192.168.10.100–199 |
| 20 | IT / Server | 192.168.20.0/24 | 192.168.20.1 | 192.168.20.100–199; Server statisch |
| 30 | Mitarbeiter | 192.168.30.0/24 | 192.168.30.1 | 192.168.30.100–199 |
| 40 | Gäste | 192.168.40.0/24 | 192.168.40.1 | 192.168.40.100–199 |

Server: `192.168.20.10/24`, Gateway `192.168.20.1`, DNS demo name `intranet.firma.test`.

## Einrichtung
1. Baue die Geräte und Verkabelung gemäß `docs/topology.png` auf.
2. Füge `configs/switch.txt` auf SW1 und `configs/router.txt` auf R1 ein.
3. Setze den Server statisch auf `192.168.20.10`, Maske `255.255.255.0`, Gateway `192.168.20.1`.
4. Server > Services: DNS aktivieren und `intranet.firma.test` → `192.168.20.10` hinzufügen; HTTP aktivieren.
5. Stelle alle PCs auf DHCP.
6. Speichere das Packet-Tracer-Lab als `Unternehmensnetzwerk_VLANs.pkt`.
7. Führe die Tests unten aus und speichere echte Screenshots in `screenshots/`.

## Sicherheitskonzept
- VLANs trennen die Abteilungen logisch.
- ACL auf dem Gäste-Interface blockiert Gäste beim Zugriff auf die internen Netze 192.168.10.0/24, 192.168.20.0/24 und 192.168.30.0/24.
- Dieses Labor ist eine Lern-Demo und keine produktionsreife Sicherheitskonfiguration.

## Verifikation
Auf dem Switch:
```text
show vlan brief
show interfaces trunk
```
Auf dem Router:
```text
show ip interface brief
show ip dhcp binding
show access-lists
```
Tests von PCs:
- Admin-PC erhält `192.168.10.x` und kann sein Gateway pingen.
- IT-PC erhält `192.168.20.x` und kann den Server `192.168.20.10` erreichen.
- Mitarbeiter-PC erhält `192.168.30.x` und kann sein Gateway pingen.
- Gast-PC erhält `192.168.40.x`; Zugriff auf interne Subnetze muss durch ACL blockiert werden.
- DNS-Auflösung `intranet.firma.test` liefert `192.168.20.10`.

## Portfolio-Checkliste
- [ ] `Unternehmensnetzwerk_VLANs.pkt` hinzugefügt
- [ ] Screenshot der vollständigen Topologie hinzugefügt
- [ ] Screenshot von `show vlan brief` hinzugefügt
- [ ] Screenshot von `show interfaces trunk` hinzugefügt
- [ ] Screenshot von DHCP-Bindings hinzugefügt
- [ ] Screenshot der ACL-Tests hinzugefügt
- [ ] Testprotokoll mit tatsächlich beobachteten Ergebnissen ausgefüllt
