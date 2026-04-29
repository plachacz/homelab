# Projekt Homelab — Infrastruktura Sieciowa

## Opis projektu
Środowisko homelab zbudowane w celu symulacji infrastruktury 
sieciowej klasy enterprise. Projekt obejmuje segmentację sieci 
VLAN, zarządzanie użytkownikami przez Active Directory, 
centralny monitoring oraz system powiadomień.

## Użyte technologie
- **pfSense** — Firewall, segmentacja VLAN, DHCP Relay
- **Windows Server 2012** — Active Directory, DNS, DHCP
- **Ubuntu Server 22.04** — Docker, Prometheus, Grafana
- **VirtualBox** — Platforma wirtualizacji

## Projekt sieci
| VLAN | Nazwa | Podsieć | Przeznaczenie |
|------|-------|---------|---------------|
| 10 | Servers | 192.168.10.0/24 | Windows Server, Ubuntu |
| 20 | Employees | 192.168.20.0/24 | Stacje robocze pracowników |
| 30 | Guests | 192.168.30.0/24 | Urządzenia gości |

## Zrealizowane funkcjonalności

### 🔒 Segmentacja sieci (VLAN)
- Trzy osobne podsieci izolujące serwery, pracowników i gości
- Reguły firewall pfSense blokujące ruch między VLAN-ami
- Goście mają dostęp tylko do internetu
- Pracownicy mają dostęp do AD/DNS ale nie do podsieci serwerów
- DHCP Relay — Windows Server centralnie zarządza adresacją IP

### 👥 Active Directory
- Domena: homelab.local
- Struktura OU: IT_Admins, Office_Users, Guests
- Grupy bezpieczeństwa z przypisanymi rolami
- Polityki GPO dostosowane do każdego działu

### 📋 Polityki GPO
| GPO | Dotyczy | Ograniczenia |
|-----|---------|--------------|
| GPO_IT_Admins | OU IT_Admins | Pełny dostęp, RDP włączony |
| GPO_Office_Users | OU Office_Users | Brak Panelu Sterowania, brak CMD |
| GPO_Guests | OU Guests | Maksymalne ograniczenia |

### 📊 Monitoring infrastruktury
- Prometheus zbiera metryki co 15 sekund
- Grafana — dashboardy dla Linux i Windows Server
- Alertmanager wysyła krytyczne alerty na Slack
- Skonfigurowane alerty: CPU > 85%, RAM > 90%, 
  Dysk < 15%, Serwer niedostępny

## Screenshoty
### Struktura Active Directory
[../screenshots/ad-structure.png]

### Reguły firewall pfSense
[../screenshots/pfsense-firewall-employees]
[../screenshots/pfsense-firewall-guests]

### Segmentacja VLAN
[../screenshots/pfsense-interfaces]

### Grafana Dashboard
[../screenshots/grafana-dashboard-linux]
[../screenshots/grafana-dashboard-windows]

### Powiadomienie Slack
[../screenshots/slack-alert-notification]

## Środowisko
Całość uruchomiona na VirtualBox z następującymi maszynami wirtualnymi:

| VM | System | Rola | IP |
|----|--------|------|----|
| pfSense | pfSense CE | Firewall / Router | 192.168.10.1 |
| DC01 | Windows Server 2012 | AD, DNS, DHCP | 192.168.10.10 |
| Ubuntu Monitor | Ubuntu Server 22.04 | Monitoring | 192.168.10.50 |
| Stacja robocza | Windows 10/11 | Klient domenowy | 192.168.20.x |
