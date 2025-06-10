# Conf_Router_ca
Configuration XML du routeur principal 

# Configuration pfSense - Router-ca.archambault.ca

Ce dépôt contient le fichier de configuration XML du pare-feu **pfSense** utilisé dans l'infrastructure réseau de l'organisation **Archambault Corp.**.

## 📄 Fichier contenu

- `config-Router-ca.archambault.ca-20250409175312.xml` : Export de la configuration complète du pare-feu `Router-ca.archambault.ca` (version pfSense 23.3).

## 🧰 Informations générales

- **Version pfSense** : 23.3
- **Nom d’hôte** : `Router-ca`
- **Domaine** : `archambault.ca`
- **Fuseau horaire** : Europe/Paris
- **Langue de l’interface** : Français (`fr_FR`)
- **Méthode d’authentification externe** : LDAP (via `SRV-AD1.archambault.ca`)

## 🌐 Interfaces réseau

| Interface | Nom        | Adresse IP         | Masque     | Rôle              |
|-----------|------------|--------------------|------------|-------------------|
| WAN       | WAN        | DHCP (via em0)     | -          | Accès internet    |
| LAN       | LAN_CLT    | 172.20.3.254       | /22        | Réseau client     |
| OPT1      | LAN_SRV    | 192.168.50.254     | /24        | Réseau serveur    |
| OPT2      | DMZ        | 10.10.20.254       | /24        | Zone démilitarisée|
| OPT3      | XMLRPC     | 10.10.10.1         | /24        | Synchronisation   |

## 🔐 Règles de filtrage (Firewall)

- **Filtrage par interface** : Règles définies par interface (LAN, DMZ, Serveur, XMLRPC…)
- **Règles notables** :
  - Autorisation ICMP depuis certains postes pour supervision.
  - Blocage du trafic ICMP sortant depuis LAN_SRV, DMZ, XMLRPC pour raisons de sécurité.
  - Ouverture des ports nécessaires pour :
    - DNS, LDAP, Kerberos, Global Catalog
    - HTTP/HTTPS (80/443)
    - SNMP, NetBIOS, SMB
  - Synchronisation PFSync et CARP via XMLRPC.

## 🔒 VPN IPSec

- **Type** : Site-à-site IKEv2
- **Tunnel principal** :
  - **Peer** : 10.101.131.100
  - **Phase 1** : AES-256 / SHA256 / DH14
  - **Phase 2** : Plusieurs sous-réseaux (LAN_SRV, DMZ, VLAN Commerce…)

## 🔁 Aliases

- `SERVER_AD` : 192.168.50.150, 192.168.50.152 (Contrôleurs de domaine)
- `GLPI` : 10.10.20.2
- `SRV_MAIL` : 10.10.20.1
- `CENTREON` : Serveur de supervision
- `SERVERS` : Liste complète des serveurs du LAN Serveur

## 🔧 Services activés

- **SNMP** : Activé, communauté : `archambault`
- **HTTPS** : Interface Web GUI sécurisée via certificat SSL
- **NTP** : Serveur : `2.pfsense.pool.ntp.org`
- **Références DNS** :
  - SRV-AD1.archambault.ca
  - SRV-AD2.archambault.ca
