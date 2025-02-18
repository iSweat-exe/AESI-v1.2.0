# 💡 AESI v1.2.0 | 19/02/2025 | Publique  

## ➕ Ajout de la commande `/all-in` ✅  
💰 **Misez tout, risquez gros !**  
- Permet de **miser l’intégralité** de son argent.  
- **Si l'on perd** : tout est perdu sauf **100 coins** pour éviter un solde à zéro.  
- **Si l'on gagne** : le gain est calculé comme **(Solde Actuel / 2)**.  

🎭 **Des probabilités équilibrées**  
Les chances de gagner ne sont pas totalement équitables pour éviter les abus et garantir une économie stable.  

---

## 🛠️ Détails techniques de la commande `/all-in` (Devs)  

### 📌 Fonctionnement de la commande  
La commande `/all-in` permet aux utilisateurs de **miser l’intégralité** de leur argent avec une **chance de 45%** de gagner.  
- **Gain possible** : 50% du solde actuel en plus.  
- **Perte** : Le joueur tombe à **100 coins** minimum pour éviter un solde négatif.  

### 📂 Fichiers modifiés  
- `commands/all-in.js` → Ajout de la commande  
- `models/profileSchema.js` → Utilisation du modèle de profil  
- `config.json` → Chargement des paramètres  
- `utils/lang.js` → Support multilingue  

---

### 🔧 Modifications et Optimisations  

#### ✅ **Ajout de la logique de pari**  
- Vérification que l’utilisateur possède **au moins 200 coins** avant de jouer.  
- Définition d’un **taux de victoire de 45%** (`winChance = 0.45`).  
- Calcul des gains/pertes :
  - **Si gagné** : Ajout de **50% du solde** à la balance.  
  - **Si perdu** : Réduction du solde à **100 coins minimum** pour éviter un solde à 0.  

#### 🛡️ **Sécurité et Anti-Abus**  
- **Impossible de jouer avec un solde inférieur à 200 coins** pour éviter de spam la commande sans risque.  
- **Taux de victoire ajusté** (45%) pour empêcher une montée trop rapide de l’économie.  
- **Utilisation de `await profileModel.findOneAndUpdate()`** pour mettre à jour la base de données de manière atomique et éviter les incohérences.  

#### 🎨 **Amélioration de l’interface utilisateur (Embed)**  
- **Utilisation d’EmbedBuilder** pour afficher les résultats de manière visuelle.  
- **Affichage dynamique** des messages :
  - 🟢 **Victoire** → Texte vert avec message encourageant.  
  - 🔴 **Défaite** → Texte rouge avec mise en garde.  
- **Ajout d’un footer** pour plus de clarté.  

#### 🚀 **Optimisation des performances**  
- **Réduction des accès à la base de données** en n'effectuant qu'un seul `findOneAndUpdate()` au lieu de plusieurs requêtes.  
- **Gestion efficace des erreurs** :  
  - Erreur lors de l’accès à la base de données → Envoi d’un message d’échec.  
  - Problème de connexion au bot → Log dans la console.

---

## ➕ Amélioration de l'embed de `/Leaderboard` ✅  
📊 **Un design plus clair et moderne !**  
- **Refonte visuelle** pour une meilleure lisibilité.  
- Affichage des **top joueurs** avec leur solde et leur classement.  
- **Indicateurs visuels** pour distinguer les meilleurs joueurs (ex: 🥇, 🥈, 🥉).  
- Ajout de la possibilité de **voir son propre classement**, même si l'on n'est pas dans le top.  

---

### 🛠️ Détails techniques pour les développeurs :  
📌 **Modifications principales**  
- **Reformatage complet** de l’embed pour s’adapter aux écrans.  
- **Ajout d’icônes dynamiques** selon le rang du joueur.  
- Correction d’un bug où **les rangs s’affichaient incorrectement** dans certaines situations.  

🔧 **Optimisations**  
- Meilleure gestion de la requête MongoDB pour récupérer les meilleurs joueurs **plus efficacement**.  
- **Suppression des appels inutiles** à l’API Discord.  

---

## ➕ Ajout de la commande `/double-or-nothing` ✅  
🎰 **Un jeu de risque et de chance !**  
- Le joueur commence avec **10 coins** et peut **doubler ses gains** à chaque tour.  
- **S’il perd un tour** : il **perd tout** et repart à zéro.  
- **Plus il monte**, plus la récompense est élevée… mais aussi le risque de tout perdre.  

🎭 **Un équilibre entre chance et psychologie**  
Les premiers tours semblent **faciles**, mais la probabilité de gagner **diminue progressivement** sans être affichée clairement.  
Des **messages de faux "gros gagnants"** apparaissent pour inciter à tenter plus haut.  

---

### 🛠️ Détails techniques pour les développeurs :  
📌 **Modifications principales**  
- Création d’un **système de jeu en escalade** avec gestion des rounds.  
- **Implémentation des probabilités dégressives** pour simuler une augmentation du risque.

🔧 **Optimisations**  
- Ajout d’un **système de cooldown** pour éviter les abus.  
- Réduction des requêtes à la base de données pour **optimiser les performances**.  

---

## ➕ Mise à jour de la commande `/stats` ✅  
📊 **Plus de données sur votre progression !**  
- Ajout des statistiques des **nouvelles commandes** : `/rps`, `/double-or-nothing`, `/work`.  

---

### 🛠️ Détails techniques pour les développeurs :  
📌 **Modifications principales**  
- Ajout d’un **suivi des nouvelles commandes** dans la base de données.  
- Affichage dynamique des **statistiques du joueur** en temps réel.  

🔧 **Optimisations**  
- Optimisation des **requêtes pour éviter une surcharge de la BDD**.  

---

## ➕ Ajout de la commande `/work` ✅  
🛠️ **Gagne de l’argent en travaillant !**  
- Permet aux joueurs de **gagner de l’argent** en effectuant un travail.  
- Une récompense aléatoire entre **10$ et 30$** est attribuée.  
- **Un cooldown de 10 secondes** empêche l’abus de la commande.  
- **Chaque utilisation** augmente le compteur de travail effectué.  

---

### 🛠️ Détails techniques pour les développeurs :  
📌 **Modifications principales**  
- Ajout d’un **système de cooldown dynamique**.  
- Stockage du **nombre de fois où un joueur a travaillé**.  

🔧 **Optimisations**  
- Gestion **optimisée des requêtes à la BDD** pour ne pas ralentir le bot.  
- Mise en place d’un **système anti-spam** pour empêcher les abus.  

---

## 🌍 Ajout du support multilingue (en cours) ✅  
🗺️ **AESI parle plusieurs langues !**  
- Le bot est désormais en **cours de traduction** en **Français**, **Anglais**, **Espagnol** et **Russe** !  
- Certaines parties ne sont **pas encore totalement traduites**, mais des mises à jour viendront compléter le **support multilingue**.  

---

### 🛠️ Détails techniques pour les développeurs :  
📌 **Modifications principales**  
- Intégration d’un **système de fichiers JSON** pour la gestion des langues.  
- Ajout d’une **commande `/config langue`** permettant aux utilisateurs de choisir leur langue.

---

## 🛠️ Ajout du Dev Mode ✅  
👨‍💻 **Un mode dédié aux développeurs**  
- Lorsque le mode est **activé**, seuls les **développeurs autorisés** peuvent **utiliser le bot**.  
- **Tous les autres membres** verront un message indiquant que le bot est **temporairement indisponible**.  

---

### 🛠️ Détails techniques pour les développeurs :  
📌 **Modifications principales**  
- Ajout d’un **flag interne** permettant d’activer/désactiver le mode développeur.  
- Sécurisation de l’accès avec une **whitelist de développeurs**.

---

## 🛠️ Ajout du PayDay ✅  
💰 **Un revenu passif pour tous !**  
Chaque 24 heures, tous les joueurs recevront automatiquement une récompense de **1300$**.  

📆 **Un rendez-vous quotidien**  
Le payday se déclenche **une fois par jour**, garantissant une économie stable.  

🎁 **Pourquoi ne pas en profiter ?**  
Accumule tes gains et grimpe dans le classement des plus riches !  

---

### 🛠️ Détails techniques pour les développeurs :  
📌 **Modifications principales**  
- Ajout d’un **événement automatique** exécuté toutes les 24 heures.  
- **Stockage de la dernière distribution** en base de données (`lastPayday`).  
- Amélioration de l'**embed d’annonce** avec un design plus lisible.  

🔧 **Optimisations**  
- Suppression des **méthodes obsolètes** pour alléger le code.  
- **Ajout de logs** pour suivre les paiements distribués.  
