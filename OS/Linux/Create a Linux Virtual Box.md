# VirtualBox - Créer une Machine Virtuelle Linux

> **Description**: Guide étape par étape pour installer et configurer une machine virtuelle Linux avec VirtualBox.

#VirtualBox #Linux #VM #Installation #Configuration

---

## 📋 Prérequis

- [ ] VirtualBox installé sur votre système
- [ ] Image ISO de la distribution Linux souhaitée
- [ ] Au moins 4GB de RAM disponible
- [ ] 20GB d'espace disque libre minimum

---

## 🔧 Installation et Configuration

### 1. Installation de VirtualBox

1. Télécharger et installer [VirtualBox](https://www.virtualbox.org)
2. Redémarrer le système si nécessaire

### 2. Préparation de l'image Linux

1. Choisir votre distribution Linux
2. Télécharger l'image ISO depuis le site officiel de la distribution
3. Vérifier l'intégrité du fichier (checksum recommandé)

### 3. Configuration de la Machine Virtuelle

#### Création de la VM

1. **Nouvelle VM** : Cliquer sur "Nouvelle"
2. **Nom et type** :
   - Nom : Choisir un nom descriptif
   - Type : Linux
   - Version : Sélectionner la distribution appropriée
3. **Mémoire** : Allouer au minimum 2GB (4GB recommandé)
4. **Disque dur** : Créer un nouveau disque virtuel (20GB minimum)

#### Configuration avancée

1. **Processeurs** : Attribuer 2+ cœurs si disponibles
2. **Image ISO** : Monter l'image dans le lecteur CD/DVD virtuel
3. **Réseau** : Configurer l'adaptateur réseau (NAT par défaut)

### 4. Installation du Système

1. **Démarrage** : Lancer la VM
2. **Installation** : Suivre l'assistant d'installation de la distribution
3. **Configuration utilisateur** : Créer un compte utilisateur avec mot de passe
4. **Redémarrage** : Redémarrer après installation complète

### 5. Post-Installation

#### Suppression de l'image ISO

1. Éteindre la VM
2. **Paramètres** → **Stockage** → Retirer l'image ISO du lecteur virtuel

#### Installation des Guest Additions

1. **Menu VM** → **Insérer l'image CD des Guest Additions**
2. Monter le CD dans le système invité
3. Exécuter l'installation des Guest Additions
4. Redémarrer la VM

![[2024-12-03_screenshot_guest_addtion.png]]

---

## 🛠️ Dépannage

### Terminal manquant

Si aucune application de console n'est disponible :

- **Paramètres** → **Applications** → **Terminal** → **Ouvrir dans le logiciel** → **Installer**

### Performances optimales

- Activer la virtualisation matérielle dans le BIOS
- Allouer suffisamment de RAM
- Installer les Guest Additions pour l'intégration complète

---

## 📚 Ressources

- [Documentation VirtualBox](https://www.virtualbox.org/manual/)
- [Liste des distributions Linux](https://distrowatch.com/)
- [[Docker]] - Alternative pour la conteneurisation
- [[Linux]] - Commandes et administration système


