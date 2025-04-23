# 🌐 Présentation de la Configuration des Switches HP ProCurve
- https://github.com/tomgillot49/Configuration-Switch-HP-Procurve/blob/master/commandes-de-bases.md
- https://github.com/tomgillot49/Configuration-Switch-HP-Procurve/blob/master/CONFIG-SWITCH.html


## 🧾 Objectif

Ce document fournit une vue d'ensemble de la configuration de base des switches HP ProCurve pour un déploiement standard en environnement réseau d'entreprise.

---

## 📦 Modèles Concernés

- HP ProCurve 2610 48 ports

---

## ⚙️ Configuration de Base

### 1. Accès au Switch

- **Console série** ou **Web Interface (si activée par défaut)**
- IP par défaut (si applicable) : `192.168.2.10`
- Identifiants par défaut :
  - **Utilisateur** : admin
  - **Mot de passe** : *(vide)*

---

### 2. Configuration IP Management

```bash
configure
vlan 1
   name "DEFAULT_VLAN"
   ip address 192.168.1.10 255.255.255.0
   exit
exit
write memory
