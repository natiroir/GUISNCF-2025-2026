# 📚 MANUEL D'UTILISATION - ANALYSETRAM©

## 🎯 **Vue d'ensemble**

**ANALYSETRAM©** est une application Java de bureau spécialisée dans l'analyse et la capture de trames de communication. Elle permet de recevoir, visualiser, filtrer et analyser des données provenant d'appareils connectés via port série (comme des ESP32) ou en mode simulation.

### **Fonctionnalités principales :**
- 📡 **Capture de trames** via port série ou simulation
- 💬 **Affichage multi-format** : binaire, hexadécimal, texte interprété
- 📈 **Visualisation graphique** des signaux 
- 🔍 **Système de filtres** colorés personnalisables
- 📖 **Dictionnaire** de traduction hexadécimal ↔ texte
- 💾 **Export de données** (CSV, JSON)
- 🗄️ **Base de données SQLite** intégrée

---

## 🚀 **Démarrage rapide**

### **Configuration système requise**
- Java 17 ou supérieur
- Système d'exploitation : Windows, macOS ou Linux
- Port série disponible (pour la capture hardware)

### **Lancement de l'application**

#### Option 1 : Via Maven
```bash
mvn clean javafx:run
```

#### Option 2 : Via JAR compilé
```bash
mvn clean package
java -jar target/demo-1.0-SNAPSHOT.jar
```

#### Option 3 : Exécution directe
```bash
java -cp target/classes org.sncf.gui.AnalyseTram
```

---

## 🖥️ **Interface utilisateur**

L'application utilise une interface **Swing** moderne avec trois vues principales accessibles via la barre d'outils supérieure :

### **🧭 Barre d'outils**
- **Logo** : `∞ ANALYSETRAM©`
- **Boutons de navigation** : 
  - 💬 **Message** : Vue principale d'affichage des trames
  - 📈 **Graphique** : Visualisation temporelle des signaux
  - 🔍 **Filtre** : Gestion des filtres personnalisés
- **Configuration série** : Sélection du port et des paramètres
- **Boutons d'action** : Envoi de configuration, écoute série

---

## 💬 **Vue Message (Vue principale)**

### **📊 Affichage des trames**
La vue message présente les données capturées sous **3 formats simultanés** :

1. **🔢 Format binaire** : Séquence de 0 et 1
   ```
   Exemple : 10110011001011100110...
   ```

2. **🔤 Format hexadécimal** : Représentation hex spacée
   ```
   Exemple : B3 2E 66 A4 FF...
   ```

3. **📝 Format texte** : Traduction intelligente
   - Via **dictionnaire personnalisé** (priorité)
   - Ou conversion **ASCII brute** pour les caractères imprimables
   ```
   Exemple : START_FRAME.data.END
   ```

### **🎛️ Boutons d'action**

#### **📖 Voir le dictionnaire**
- Ouvre une fenêtre de gestion du dictionnaire hex ↔ texte
- **Fonctionnalités** :
  - Visualisation des entrées existantes
  - Ajout de nouvelles correspondances
  - Suppression d'entrées sélectionnées
- **Format d'entrée** : `4A 5E 34 6C` → `DEBUT_TRAME`

#### **🔄 Réinitialiser**
- Efface toutes les trames affichées
- Remet à zéro les compteurs
- **Attention** : Les données restent en base

#### **🗑️ Vider la base**
- **Action destructive** : Supprime définitivement toutes les trames de la base SQLite
- Demande confirmation utilisateur
- Irréversible

#### **📤 Exporter**
Ouvre un dialogue d'export avec plusieurs options :

**Formats disponibles :**
- **CSV** : Compatible Excel/LibreOffice
  ```csv
  Timestamp,Bits,Hex,Text
  2024-01-15 10:30:45,10110011,B3,START
  ```
- **JSON** : Format structuré
  ```json
  [{"timestamp":"2024-01-15 10:30:45","bits":"10110011","hex":"B3","text":"START"}]
  ```

**Options d'export :**
- Toutes les trames ou période spécifique
- Avec ou sans filtres appliqués
- Choix du séparateur CSV

#### **🔀 Mode Simulation**
- **Bouton toggle** : Active/désactive la simulation
- En mode simulation : Génère automatiquement des trames aléatoires
- Utile pour les tests sans hardware connecté
- **Fréquence** : 1 trame/seconde

### **🎨 Filtres visuels**
Les filtres définis dans la vue **Filtre** s'appliquent automatiquement :
- **Coloration du texte** selon les motifs
- **Mise en évidence** des correspondances
- Application sur les 3 formats (bits, hex, texte)

---

## 📈 **Vue Graphique**

### **📊 Visualisation temporelle**
- **Graphique en temps réel** des signaux reçus
- **Axe X** : Temps de réception
- **Axe Y** : Valeurs des bits (0/1)
- **Synchronisation** automatique avec la vue Message

### **🎛️ Contrôles**
- **Zoom** : Molette de la souris
- **Défilement** : Barres de défilement
- **Réinitialisation** : Via le bouton "Réinitialiser" de la barre d'outils

### **🔍 Filtres graphiques**
- Application des filtres couleur sur le graphique
- **Légende** dynamique selon les filtres actifs
- **Performance** : Gestion optimisée de grandes quantités de données

---

## 🔍 **Vue Filtres**

### **📝 Gestion des filtres personnalisés**

#### **➕ Création d'un filtre**
1. Cliquer sur **"+ Nouveau filtre"**
2. **Remplir le formulaire** :
   - **Nom** : Identifiant du filtre (ex: "Trames d'erreur")
   - **Motif** : Expression à rechercher
     - Bits : `10110011` ou `*0011*` (wildcards)
     - Hex : `B3 2E` ou `FF * *` 
     - Texte : `ERROR` ou `START*`
   - **Couleur** : Sélection via palette
3. **Valider** : Le filtre est sauvegardé en base

#### **✏️ Édition d'un filtre**
- Clic sur l'icône **📝** de modification
- Modification de tous les paramètres
- Sauvegarde automatique

#### **🗑️ Suppression d'un filtre**
- Clic sur l'icône **🗑️** de suppression
- Confirmation requise
- Suppression définitive de la base

#### **🔍 Recherche de filtres**
- **Barre de recherche** en temps réel
- Filtrage par nom ou motif
- Sensible à la casse

#### **⚡ Activation/Désactivation**
- **Checkbox** pour chaque filtre
- Activation/désactivation sans suppression
- État mémorisé entre les sessions

### **🎨 Types de motifs supportés**

#### **Motifs binaires**
```
Exact: 10110011
Wildcard: 1011****0011
Début: 1011*
Fin: *0011
```

#### **Motifs hexadécimaux**
```
Exact: B3 2E 66
Wildcard: B3 * 66
Séquence: FF AA BB CC
```

#### **Motifs texte**
```
Exact: ERROR
Contient: *ERROR*
Commence: START*
Finit: *END
```

### **📚 Aide contextuelle**
- **Section d'aide** en bas de l'écran
- **Exemples** de motifs valides
- **Conseils** d'utilisation

---

## 🔌 **Configuration Série**

### **🎛️ Sélecteur de port**
Accessible via la barre d'outils droite :

#### **🔍 Détection automatique**
- **Scan** automatique des ports série disponibles
- **Bouton de rafraîchissement** (⟳)
- **Description** des ports (VID/PID)

#### **⚙️ Configurations prédéfinies**
- **Menu déroulant** "Configuration Port ▼"
- **Configurations sauvegardées** en base SQLite
- **Paramètres** : Baudrate, Parité, Bits de données, Bits d'arrêt

#### **➕ Gestion des configurations**
Via le menu configuration :

**✏️ Ajouter une configuration :**
1. Clic sur **"Ajouter configuration"**
2. Remplir les paramètres :
   - **Baudrate** : 9600, 19200, 38400, 57600, 115200...
   - **Parité** : Aucune, Paire, Impaire
   - **Bits de données** : 7, 8
   - **Bits d'arrêt** : 1, 2
3. Sauvegarde automatique

**📝 Modifier une configuration :**
- Sélection dans la liste
- Modification des paramètres
- Mise à jour en base

**🗑️ Supprimer une configuration :**
- Confirmation requise
- Suppression de la base

### **📡 Communication série**

#### **📤 Envoi de configuration**
1. **Sélectionner** un port série
2. **Choisir** une configuration
3. **Cliquer** sur "Envoyer"
4. **Attendre** la confirmation ESP32 (`READY_TO_SNIFF`)

#### **👂 Écoute des trames**
1. **Cliquer** sur "Écouter" après envoi de configuration
2. **Réception automatique** des trames
3. **Sauvegarde** automatique en base
4. **Affichage** temps réel dans les vues

#### **⏹️ Arrêt de l'écoute**
- **Clic** sur "Arrêter l'écoute"
- **Timeout automatique** après 10s d'inactivité
- **Fermeture** propre du port série

### **🔧 Protocole ESP32**
L'application communique avec des ESP32 via protocole série :

**📤 Envoi :**
```
BAUDRATE:115200
PARITY:NONE
DATABITS:8
STOPBITS:1
START_SNIFFING
```

**📥 Réception :**
```
READY_TO_SNIFF
10110011001011100110...
01001101110010110011...
```

---

## 💾 **Base de données**

### **🗃️ Structure SQLite**
L'application utilise une base SQLite locale avec 4 tables :

#### **📊 `frame_capture`**
```sql
CREATE TABLE frame_capture (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    raw_bits TEXT,      -- Données binaires
    raw_hexa TEXT,      -- Représentation hex
    raw_text TEXT,      -- Texte interprété
    timestamp TEXT NOT NULL
);
```

#### **⚙️ `port_config`**
```sql
CREATE TABLE port_config (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    baudrate TEXT,
    parity TEXT,
    databits TEXT,
    stopbits TEXT
);
```

#### **🎨 `custom_filter`**
```sql
CREATE TABLE custom_filter (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    color TEXT,
    name TEXT,
    pattern TEXT
);
```

#### **📖 `dictionary`**
```sql
CREATE TABLE dictionary (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    description TEXT NOT NULL,
    hex_pattern TEXT NOT NULL
);
```

### **📂 Emplacement de la base**
La base de données est créée automatiquement selon l'OS :

- **🪟 Windows** : `%APPDATA%\AnalyseTram\bdd.db`
- **🍎 macOS** : `~/Library/Application Support/AnalyseTram/bdd.db`
- **🐧 Linux** : `~/.local/share/AnalyseTram/bdd.db`

### **🔧 Maintenance**
- **Initialisation** automatique au premier lancement
- **Migration** depuis une base modèle si disponible
- **Sauvegarde** recommandée régulière du fichier `.db`

---

## 🔧 **Cas d'usage avancés**

### **🔬 Analyse de protocoles**

#### **1. Identification de trames**
1. **Capturer** des données via port série
2. **Analyser** les motifs récurrents
3. **Créer des filtres** pour les identifier
4. **Enrichir le dictionnaire** avec les correspondances

#### **2. Débogage de communication**
1. **Mode simulation** pour tester sans hardware
2. **Filtres d'erreur** pour isoler les problèmes
3. **Export des données** pour analyse externe
4. **Visualisation graphique** pour patterns temporels

#### **3. Documentation de protocoles**
1. **Capture** de séquences complètes
2. **Annotation** via dictionnaire
3. **Export documenté** avec filtres explicites
4. **Validation** par re-simulation

### **🧪 Tests et validation**

#### **Simulation reproductible**
- **Mode simulation** : Génération contrôlée
- **Filtres de validation** : Vérification automatique
- **Exports de référence** : Comparaisons

#### **Régression testing**
- **Capture de références** : Sessions de test validées
- **Comparaison automatique** : Via exports CSV
- **Détection d'anomalies** : Filtres d'alerte

---

## ⚠️ **Dépannage**

### **🔌 Problèmes de port série**

#### **Port non détecté**
- **Vérifier** la connexion physique
- **Rafraîchir** la liste (bouton ⟳)
- **Permissions** : Droits d'accès au port (Linux/macOS)
- **Pilotes** : Installation des drivers USB/série

#### **Échec de connexion**
- **Port occupé** : Fermer autres applications série
- **Paramètres incorrects** : Vérifier baudrate/parité
- **Timeout** : Augmenter les délais d'attente

#### **ESP32 ne répond pas**
- **Reset** de l'ESP32
- **Firmware** : Vérifier le programme ESP32
- **Protocole** : Respecter la séquence d'initialisation

### **💾 Problèmes de base de données**

#### **Erreur d'initialisation**
- **Permissions** : Droits d'écriture dans le dossier
- **Espace disque** : Vérifier l'espace disponible
- **Corruption** : Supprimer et relancer (perte de données)

#### **Performance dégradée**
- **Taille de base** : Vider périodiquement (`Vider la base`)
- **Index** : Reconstruction automatique
- **Mémoire** : Augmenter la heap Java (`-Xmx2g`)

### **🖥️ Problèmes d'interface**

#### **Affichage lent**
- **Filtres complexes** : Simplifier les motifs
- **Volume de données** : Réinitialiser périodiquement
- **Java** : Mettre à jour la JVM

#### **Freezes/Blocages**
- **Thread UI** : Fermer/relancer l'application
- **Port série** : Débrancher/rebrancher
- **Mémoire** : Surveiller l'utilisation RAM

---

## 📋 **Raccourcis et astuces**

### **⌨️ Raccourcis clavier**
*(Non implémentés par défaut - possibilité d'extension)*

### **🎯 Astuces d'utilisation**

#### **Filtres efficaces**
- **Wildcards** : Utiliser `*` pour patterns flexibles
- **Couleurs distinctes** : Faciliter l'identification visuelle
- **Noms explicites** : Documentation des filtres

#### **Performance**
- **Simulation modérée** : Éviter de laisser tourner indéfiniment
- **Exports ciblés** : Filtrer avant d'exporter
- **Base légère** : Vider régulièrement les anciennes données

#### **Organisation**
- **Configurations nommées** : Port configs explicites
- **Dictionnaire structuré** : Regrouper par protocole
- **Filtres hiérarchiques** : Du général au spécifique

---

## 📞 **Support et contact**

### **🐛 Signalement de bugs**
En cas de problème :
1. **Reproduire** le bug de manière consistante
2. **Logs** : Consulter la console Java
3. **Environnement** : Préciser OS, Java version, hardware
4. **Données** : Exporter un échantillon si pertinent

### **💡 Demandes d'évolution**
- **Fonctionnalités manquantes**
- **Améliorations d'interface**
- **Optimisations de performance**
- **Nouveaux formats d'export**

### **📚 Documentation**
- **Code source** : Javadoc intégrée
- **Architecture** : Documentation technique disponible
- **Exemples** : Cas d'usage dans les tests

---

## 📄 **Licence et crédits**

**ANALYSETRAM©** - Application d'analyse de trames série
- **Technologies** : Java 17+, Swing, SQLite, jSerialComm
- **Compatibilité** : Windows, macOS, Linux
- **Version** : 1.0-SNAPSHOT

---

*Dernière mise à jour : Janvier 2024* 