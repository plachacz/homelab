\# Reguły Firewall — pfSense



\## VLAN 20 — Employees

| Kolejność | Akcja | Źródło | Cel | Opis |

|-----------|-------|--------|-----|------|

| 1 | Pass | 192.168.20.0/24 | 192.168.10.10/32 | Dostęp do AD/DNS |

| 2 | Block | 192.168.20.0/24 | 192.168.10.0/24 | Blokada sieci serwerów |

| 3 | Block | 192.168.20.0/24 | 192.168.30.0/24 | Blokada sieci gości |

| 4 | Pass | 192.168.20.0/24 | any | Dostęp do internetu |



\## VLAN 30 — Guests

| Kolejność | Akcja | Źródło | Cel | Opis |

|-----------|-------|--------|-----|------|

| 1 | Block | 192.168.30.0/24 | 192.168.10.0/24 | Blokada sieci serwerów |

| 2 | Block | 192.168.30.0/24 | 192.168.20.0/24 | Blokada sieci pracowników |

| 3 | Pass | 192.168.30.0/24 | any | Tylko internet |



\## Wyniki testów segmentacji

| Test | Źródło | Cel | Wynik |

|------|--------|-----|-------|

| Pracownik → internet | 192.168.20.x | 8.8.8.8 | ✅ Działa |

| Pracownik → AD/DNS | 192.168.20.x | 192.168.10.10 | ✅ Działa |

| Pracownik → serwery | 192.168.20.x | 192.168.10.0/24 | ❌ Zablokowany |

| Pracownik → goście | 192.168.20.x | 192.168.30.0/24 | ❌ Zablokowany |

| Gość → internet | 192.168.30.x | 8.8.8.8 | ✅ Działa |

| Gość → serwery | 192.168.30.x | 192.168.10.0/24 | ❌ Zablokowany |

| Gość → pracownicy | 192.168.30.x | 192.168.20.0/24 | ❌ Zablokowany |



\## Screenshoty

!\[Reguły EMPLOYEES](../screenshots/pfsense-firewall-employees.png)

!\[Reguły GUESTS](../screenshots/pfsense-firewall-guests.png)

