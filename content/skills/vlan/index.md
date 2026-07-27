+++
title = "VLAN & trunking (802.1Q) – szczegółowy opis"
date = 2026-01-10
type = "skill"
category = "Sieci"
level = 85
related_skill_slug = "vlan"
description = "Szczegółowy opis umiejętności: VLAN, trunking, 802.1Q, konfiguracja na urządzeniach sieciowych."
tags = ["sieci", "vlan", "802.1q", "trunking"]
draft = true
+++

## Czym jest VLAN?

**VLAN (Virtual Local Area Network)** to technologia umożliwiająca podział jednej fizycznej sieci LAN na wiele logicznie izolowanych sieci. Urządzenia w różnych VLAN-ach nie widzą się nawzajem, chyba że użyjemy routera (routing między VLAN-ami).

## Standard 802.1Q

IEEE 802.1Q to standard określający sposób tagowania ramek Ethernet w celu identyfikacji przynależności do VLAN-u. Tag 802.1Q ma 4 bajty i zawiera:

- **Tag Protocol Identifier (TPID)**: 0x8100 (2 bajty)
- **Tag Control Information (TCI)**:
  - Priority Code Point (PCP): 3 bity (priorytet QoS)
  - Drop Eligible Indicator (DEI): 1 bit
  - VLAN ID (VID): 12 bitów (0-4095, VID 0 i 4095 zarezerwowane)

## Typy portów

### Access Port
- Należy do jednego VLAN-u
- Ramki wychodzące są **bez tagu** (untagged)
- Używany do podłączania urządzeń końcowych (komputery, drukarki)

### Trunk Port
- Przesyła ramki z **tagiem VLAN** (tagged)
- Obsługuje wiele VLAN-ów jednocześnie
- Używany do połączeń między przełącznikami lub przełącznik-router

### Native VLAN
- Na trunk porcie, ramki z native VLAN idą **bez tagu**
- Domyślnie to VLAN 1 (bezpieczniej zmienić)

## Doświadczenie praktyczne

W swojej pracy konfigurowałem VLAN-y na:
- **MikroTik RouterOS** – CRS326, CCR1036, hAP ac²
- **Cisco IOS** – Catalyst 2960, 3750
- **OPNsense** – interfejsy VLAN, routing między VLAN-ami
- **Proxmox VE** – VLAN-aware bridge dla maszyn wirtualnych

## Przykładowa konfiguracja

### MikroTik (RouterOS v7)

```bash
/interface bridge vlan add bridge=bridge1 tagged=ether1 untagged=ether2,ether3 vlan-ids=10
```

### Cisco IOS

```bash
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
 switchport trunk native vlan 99
```

## Podsumowanie

VLAN-y to podstawowe narzędzie każdego inżyniera sieciowego. Umiejętność poprawnej konfiguracji portów access/trunk, zrozumienie tagowania 802.1Q oraz routingu między VLAN-ami jest kluczowa przy budowie nowoczesnych sieci.