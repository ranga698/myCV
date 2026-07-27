+++
title = "Jak skonfigurować VLAN na przełączniku MikroTik"
date = 2026-01-15
type = "post"
description = "Praktyczny przewodnik krok po kroku: konfiguracja VLAN-ów na switchu MikroTik CRS326, trunking, access porty i testowanie łączności między VLAN-ami."
tags = ["sieci", "mikrotik", "vlan", "konfiguracja"]
draft = true
+++

## Wstęp

VLAN (Virtual LAN) to technologia pozwalająca na logiczne podzielenie jednej fizycznej sieci na wiele izolowanych sieci logicznych. W tym artykule pokażę, jak skonfigurować VLAN-y na przełączniku MikroTik CRS326.

## Topologia

Założenia:
- Switch MikroTik CRS326-24G-2S+
- VLAN 10: sieć administracyjna (192.168.10.0/24)
- VLAN 20: sieć dla gości (192.168.20.0/24)
- Port 1: trunk do routera
- Porty 2-10: access porty VLAN 10
- Porty 11-20: access porty VLAN 20

## Konfiguracja krok po kroku

### 1. Połączenie z urządzeniem

Połącz się przez SSH lub WinBox:

```bash
ssh admin@192.168.88.1
```

### 2. Utworzenie VLAN-ów

```bash
/interface vlan add name=VLAN10 vlan-id=10 interface=bridge1
/interface vlan add name=VLAN20 vlan-id=20 interface=bridge1
```

### 3. Konfiguracja portów access

Dla VLAN 10 (porty 2-10):
```bash
/interface bridge vlan add bridge=bridge1 tagged=ether1 untagged=ether2,ether3,ether4,ether5,ether6,ether7,ether8,ether9,ether10 vlan-ids=10
```

### 4. Konfiguracja trunk portu

Port 1 jako trunk z oznaczonymi VLAN-ami:
```bash
/interface bridge vlan add bridge=bridge1 tagged=ether1 untagged=none vlan-ids=10,20
```

## Testowanie

Po konfiguracji sprawdź łączność:
```bash
/ping 192.168.10.1
/ping 192.168.20.1
```

## Podsumowanie

Konfiguracja VLAN-ów na MikroTik jest prosta i intuicyjna. Kluczowe jest zrozumienie różnicy między portami access a trunk oraz prawidłowe oznaczenie VLAN-ów na bridge.