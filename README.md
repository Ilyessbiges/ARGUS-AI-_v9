# ⬡ ARGUS-AI IDS v9.0 — Cognitive Defence Platform

> Système de détection d'intrusion réseau intelligent pour environnements professionnels (Kali Linux)

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![Platform](https://img.shields.io/badge/Platform-Kali%20Linux-red)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

---

## 📌 Description

ARGUS-AI est un IDS (Intrusion Detection System) réseau temps réel conçu pour la détection,
l'analyse et la réponse automatisée aux menaces. Il combine plusieurs couches d'intelligence :
analyse de paquets, machine learning, threat intelligence, et visualisation en temps réel.

**Conçu pour :** SOC, CERT, pentesteurs défensifs, chercheurs en cybersécurité.

---

## 🧩 Fonctionnalités

### Détection réseau
| Module | Description |
|--------|-------------|
| Port Scan | SYN, NULL, XMAS, UDP, horizontal/vertical |
| SSH Brute-Force | Comptage tentatives, fingerprint d'outils |
| HTTP Anomaly | SQLi, XSS, Path Traversal, RCE, scanners |
| DNS Tunneling | Entropie, labels longs, TXT flood |
| Lateral Movement | SMB, RDP, WMI, Kerberoasting |
| Data Exfiltration | Volume sortant, base64, compression |
| CVE Matcher | Log4Shell, EternalBlue, ProxyLogon, etc. |
| ARP Spoofing | Détection cache ARP |
| YARA Scanner | Matching de signatures personnalisées |
| Buffer Overflow | NOP sled, haute entropie, taille anormale |

### Wi-Fi / 802.11
| Détection | Description |
|-----------|-------------|
| Evil Twin AP | Faux point d'accès imitant un SSID légitime |
| Deauth Flood | Attaque de désauthentification 802.11 |
| PMKID Capture | Tentative de capture pour cracking WPA2 |
| WPA2 Handshake | Détection capture EAPOL 4-way |
| Probe Flood | Scan passif de réseaux Wi-Fi |

### Intelligence artificielle
- **LightGBM** : scoring de risque ML en temps réel
- **RL Agent** : agent de réponse par renforcement (allow/throttle/block/isolate/honeypot)
- **DGA Detector** : détection de domaines générés algorithmiquement
- **LLM Analyzer** : analyse contextuelle GPT-4o-mini (optionnel)
- **Isolation Forest** : détection d'anomalies non supervisée

### Infrastructure
- **Zeek** : consommateur de logs Zeek (DNS, SSL, HTTP, SSH, conn, files, weird)
- **eBPF** : sensor kernel-space haute performance
- **GeoIP** : géolocalisation des IPs suspectes
- **Threat Intel** : feeds IOC automatiques + fichier local
- **SQLite/PostgreSQL** : persistance des alertes
- **PCAP Export** : capture automatique des paquets suspects
- **Dashboard** : interface web temps réel (Flask + Socket.IO + Chart.js)
- **Rapport HTML** : génération automatique quotidienne

---

## 🖥️ Prérequis

- Kali Linux (ou Debian/Ubuntu)
- Python 3.10+
- 2 interfaces Wi-Fi (wlan0 + wlan1 pour le mode monitor)
- Droits root (`sudo`)

---

## ⚙️ Installation

```bash
# 1. Cloner le dépôt
git clone https://github.com/votre-user/argus-ai-ids.git
cd argus-ai-ids

# 2. Installer les dépendances
pip install -r requirements.txt

# 3. (Optionnel) Compiler l'extension Cython pour de meilleures performances
python3 argus_ids_v9_kali.py --gen-cython
python3 setup_argus.py build_ext --inplace

# 4. (Optionnel) Télécharger la base GeoIP
mkdir -p data
# Télécharger GeoLite2-City.mmdb depuis https://www.maxmind.com
# et le placer dans data/GeoLite2-City.mmdb
```

---

## 🚀 Utilisation

```bash
# Mode simulation (sans interfaces physiques, idéal pour tests)
sudo python3 argus_ids_v9_kali.py --simulate

# Mode production wlan0 uniquement
sudo python3 argus_ids_v9_kali.py --wlan0 wlan0

# Mode complet avec monitor Wi-Fi
sudo python3 argus_ids_v9_kali.py --wlan0 wlan0 --wlan1 wlan1 --monitor

# Avec blocage automatique des IPs malveillantes
sudo python3 argus_ids_v9_kali.py --wlan0 wlan0 --auto-block

# Avec LightGBM et RL Agent activés
sudo python3 argus_ids_v9_kali.py --wlan0 wlan0 --enable-rl --enable-lgbm

# Changer le port du dashboard (défaut: 5000)
sudo python3 argus_ids_v9_kali.py --simulate --port 8080
```

---

## 📊 Dashboard

Accéder au dashboard temps réel dans le navigateur :

```
http://localhost:5000
```

**Fonctionnalités du dashboard :**
- Feed d'alertes temps réel
- Top attaquants / géolocalisation
- Statistiques par sévérité, type, source
- Kill chain tracking
- Statut des interfaces et modules
- Score LGBM moyen

---

## 🔌 API REST

| Endpoint | Description |
|----------|-------------|
| `GET /api/alerts` | Dernières alertes (param: `n`, `severity`) |
| `GET /api/stats` | Statistiques 24h |
| `GET /api/attackers` | Top attaquants |
| `GET /api/blocked` | IPs bloquées actives |
| `GET /api/ioc/stats` | Statistiques IOC |
| `POST /api/ioc/add` | Ajouter un IOC manuellement |
| `GET /api/pcap/list` | Liste des captures PCAP |
| `GET /api/report/generate` | Générer un rapport HTML |
| `GET /api/export/csv` | Exporter les alertes en CSV |

---

## 📁 Structure du projet

```
argus-ai-ids/
├── argus_ids_v9_kali.py    # Script principal
├── requirements.txt
├── README.md
├── config/
│   └── argus_config.yaml   # Configuration (auto-généré au premier lancement)
├── rules/
│   └── yara_rules.yar      # Règles YARA personnalisées
├── data/
│   ├── ioc_list.txt        # IOC locaux (ip:x.x.x.x / domain:evil.com)
│   └── GeoLite2-City.mmdb  # Base GeoIP (à télécharger)
├── models/                 # Modèles ML (non versionnés)
└── output/                 # Logs, PCAP, rapports (non versionné)
    ├── argus.db
    ├── siem_export.jsonl
    ├── pcap/
    ├── reports/
    └── incidents/
```

---

## 🛡️ Ajouter des IOC personnalisés

Éditer `data/ioc_list.txt` :

```
# Format : type:valeur
ip:192.168.1.100
domain:malware.example.com
hash:d41d8cd98f00b204e9800998ecf8427e
```

Ou via l'API :
```bash
curl -X POST http://localhost:5000/api/ioc/add \
  -H "Content-Type: application/json" \
  -d '{"type":"ip","value":"1.2.3.4"}'
```

---

## ⚙️ Configuration

Le fichier `config/argus_config.yaml` est généré automatiquement.
Principaux paramètres :

```yaml
interfaces:
  wlan0: wlan0
  wlan1: wlan1
  auto_block: false       # Bloquer automatiquement les IPs suspectes
  learning_mode: false    # Mode apprentissage (pas de réponse automatique)

lgbm:
  enabled: true
  risk_thresholds:
    MONITOR: 0.3
    ALERT: 0.55
    BLOCK_IP: 0.75
    ISOLATE: 0.90

wifi:
  deauth_threshold: 10
  evil_twin_detect: true
  channel_hop: true
```

---

## ⚠️ Avertissement légal

Ce logiciel est destiné **exclusivement** à :
- La surveillance de **votre propre infrastructure**
- Les environnements de **lab isolés**
- Les missions de sécurité **avec autorisation écrite**

Toute utilisation sur des systèmes tiers sans autorisation est **illégale**.
Les auteurs déclinent toute responsabilité en cas d'usage non autorisé.

---

## 📄 Licence

MIT License — voir [LICENSE](LICENSE)

---

## 👤 Auteur

Projet développé dans le cadre de recherche en cybersécurité défensive.
