# 🌐 Présentation de la Configuration des Switches HP ProCurve

## 🧾 Objectif

Ce document fournit une vue d'ensemble de la configuration de base des switches HP ProCurve pour un déploiement standard en environnement réseau d'entreprise.

---

## 📦 Modèles Concernés

- HP ProCurve 1810G
- HP ProCurve 2510
- HP ProCurve 2530
- HP ProCurve 2920
- HP ProCurve 3500yl

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
