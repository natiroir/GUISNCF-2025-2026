# GUISNCF-2025-2026

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=java&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-C71A36?style=for-the-badge&logo=apache-maven&logoColor=white)
![SNCF](https://img.shields.io/badge/Projet-SNCF-0088CE?style=for-the-badge)

## 📋 Description

L'objectif fondamental de ce projet est de développer un outil capable d’analyser et de décrire les trames circulant sur un bus **RS232 ou RS485**. 

L'application permet :
- D'afficher les flux de données de manière lisible et structurée.
- De fonctionner en **détection automatique** : aucune connaissance préalable des paramètres de configuration de la ligne (baudrate, parité, stop bits, etc.) n'est requise.
- De faciliter le diagnostic et la maintenance des systèmes de communication série de la SNCF.

## 🚀 Fonctionnalités

- **Auto-détection** : Scan et identification automatique des paramètres de communication série.
- **Analyse en temps réel** : Capture et décodage des trames RS232/485.
- **Interface Graphique (GUI)** : Visualisation simplifiée des données pour les opérateurs.
- **Historisation** : Base de données locale (SQLite) pour l'enregistrement des sessions.

## 🛠️ Technologies utilisées

- **Langage** : Java
- **Gestionnaire de dépendances** : Maven
- **Base de données** : SQLite (fichier `bdd.db`)
- **Interface** : JavaFX / Swing (selon implémentation)

## 📦 Installation et Lancement

### Prérequis
- Java JDK 17 ou supérieur
- Maven

### Installation
1. Cloner le dépôt :
   ```bash
   git clone [https://github.com/natiroir/GUISNCF-2025-2026.git](https://github.com/natiroir/GUISNCF-2025-2026.git)
   cd GUISNCF-2025-2026
