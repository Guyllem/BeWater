# Be Water - Brainstorming & Design Document

**Date**: 2026-01-08
**Type**: Application mobile Flutter (iOS & Android)
**Objectif**: Application de rappel pour boire de l'eau avec une approche contraignante mais non intrusive

---

## 🎯 Concept Principal

Une application de rappel d'hydratation qui se différencie par une fonctionnalité unique : **la pause automatique des médias de divertissement** pour forcer l'action, plutôt qu'une simple notification passive qu'on peut ignorer.

---

## ✨ Fonctionnalités Principales (MVP Feature-Complete)

### 1. Interface Principale - La Goutte Animée

**Écran principal** :
- Une grosse goutte d'eau centrale avec **physique liquide animée**
- La goutte se vide progressivement selon le temps restant avant le prochain rappel
- Animation réaliste : l'eau bouge à l'intérieur avec une logique physique
- **Timer affiché en dessous** de la goutte (format clair : "1h 23min restantes")
- État : **Goutte pleine** = vient de boire / **Goutte vide** = il faut boire maintenant

**Animation in-app** :
- À 1-2 minutes avant le rappel, si l'app est ouverte, la goutte "s'excite" (vibration visuelle, changement d'animation)

---

### 2. Système de Rappels & Notifications

**Pré-notification douce** (3 min avant) :
- Notification discrète : "Dans 3 min, il sera temps de boire 💧"
- Permet à l'utilisateur d'anticiper

**Notification principale** (à l'heure H) :
- Si aucune action après la pré-notification
- Notification avec boutons d'action :
  - "J'ai bu" → confirme et réactive les médias
  - "Rappelle-moi dans 10 min" (snooze)

**Pause automatique des médias** :
- Lors de la notification principale, **pause des médias de divertissement** :
  - Musique (Spotify, Apple Music, etc.)
  - Vidéos (YouTube, etc.)
  - Podcasts
  - Tout média de divertissement
- **NE PAS toucher** :
  - Appels téléphoniques
  - Navigation GPS
  - Autres médias critiques

**Confirmation "J'ai bu"** :
- Disponible de 2 façons :
  1. **Via la notification** (bouton d'action rapide)
  2. **Dans l'app** (bouton sur l'écran principal)
- Après confirmation → **Reprise automatique des médias**
- Reset du timer et la goutte se remplit

**Snooze** :
- Option "Rappelle-moi dans 10 min"
- Disponible dans l'app ET dans les notifications
- **Maximum 3 snoozes consécutifs** ✅ *décidé* — au-delà, le rappel est marqué 'missed' et le prochain se déclenche normalement

**Rappel ignoré** ✅ *décidé* :
- Si l'utilisateur ignore la notification (sans snooze, sans confirmation) → rappel marqué 'missed'
- Le prochain rappel se déclenche normalement selon le planning

---

### 3. Onboarding & Configuration Initiale

**Premier lancement - Onboarding complet** :
1. Écran de bienvenue
2. Collecte d'informations :
   - **Poids** (pour calcul des besoins en eau)
   - **Niveau d'activité** (sédentaire, modéré, actif, très actif)
   - **Horaires de sommeil** (pour ne pas déranger la nuit)
   - **Quantité estimée par verre** (200ml, 300ml, 500ml, personnalisé)
3. Configuration automatique d'un planning intelligent
4. Possibilité de modifier ensuite dans les paramètres

**Algorithme de calcul de la quantité d'eau quotidienne** ✅ *décidé* :
- Source : guidelines ESPEN (nutrition clinique) + EFSA
- Formule : `35ml × poids(kg) × facteur_activité`
- Facteurs d'activité : Sédentaire ×1.0 / Modéré ×1.2 / Actif ×1.4 / Très actif ×1.6
- Plancher : 1500ml/jour — Plafond : 4000ml/jour (sécurité)
- Exemple : 70kg, actif = 70 × 35 × 1.4 = 3430ml/jour
- Intervalle entre rappels = `total_ml ÷ verre_ml` rappels répartis sur la plage horaire active

**Mode nuit** ✅ *décidé* :
- Silence total pendant les heures de sommeil : aucun rappel, aucune notification, aucune pause audio
- La goutte ne se vide pas pendant le mode nuit
- Premier rappel déclenché dès la fin de la plage de sommeil

**Modes de configuration des rappels** (hybride et flexible) :
- **Mode intelligent par défaut** : suggestions basées sur le profil (poids, activité)
- **Profils prédéfinis** : "Léger" (3h), "Normal" (2h), "Intensif" (1h)
- **Personnalisation complète** :
  - Intervalles personnalisés
  - Plages horaires spécifiques (ex: toutes les 1h30 en journée, pause de 22h-7h)
  - Modification manuelle à tout moment

---

### 4. Statistiques & Tracking

**Type de tracking** :
- **Régularité** : Taux de respect des rappels (ex: "8/10 rappels respectés aujourd'hui - 80%")
- **Quantité estimée** :
  - Basée sur la quantité par verre définie par l'utilisateur
  - Si non renseignée : calcul automatique basé sur le profil (poids, etc.)
  - **Ne pas demander à chaque fois** - estimation automatique
  - Modifiable dans les paramètres

**Métriques affichées** :
- Graphiques de régularité sur 7 jours / 30 jours
- Tendances (heures où tu bois le plus/moins)
- Nombre de snooze utilisés
- Streaks ("5 jours d'affilée !")
- Estimation du volume total quotidien/hebdomadaire

**Gamification - Badges/Succès** :
- Système de badges simples à débloquer
- Exemples :
  - "Première semaine complète"
  - "30 jours sans manquer un rappel"
  - "100 verres d'eau"
  - "Early bird" (boire dès le premier rappel du matin 7 jours de suite)
- Affichage dans une section dédiée
- Pas de système de niveaux complexe, juste des succès à collectionner

---

### 5. Paramètres & Personnalisation

**Paramètres disponibles** :
- Profil utilisateur (poids, activité, quantité par verre)
- Mode de rappel (intelligent, profil prédéfini, personnalisé)
- Plages horaires et intervalles personnalisés
- Horaires de sommeil / mode nuit
- Préférences de notification (son, vibration - configurable)
- Comportement des médias (activer/désactiver la pause auto)
- Langue et unités (ml, oz)

---

## 🛠️ Stack Technique & Architecture

### Framework & Plateformes
- **Flutter** (Dart) pour iOS et Android (cross-platform)
- **Minimum SDK** : iOS 12+, Android 8.0+ (API 26+)

### Base de données
- **SQLite** (via sqflite package) pour le stockage local
- Pas de backend nécessaire pour le MVP
- Toutes les données restent sur le téléphone de l'utilisateur

### Packages principaux
- **sqflite** : Base de données locale
- **flutter_local_notifications** : Notifications locales
- **workmanager** : Tâches en arrière-plan
- **audio_session** (iOS) + **audio_manager** (Android) : Contrôle audio/médias
- **permission_handler** : Gestion des permissions
- **flutter_riverpod** : Gestion d'état ✅ *décidé*
- **go_router** : Navigation ✅ *décidé*
- **fl_chart** : Graphiques pour les statistiques
- **shared_preferences** : Préférences simples

### Pattern d'architecture : MVVM Feature-first ✅ *décidé*

Chaque feature contient sa propre View, ViewModel (Riverpod Notifier) et State.
Les repositories font le pont entre les ViewModels et SQLite.

```
lib/
├── main.dart
├── core/
│   ├── database/          # SQLite setup, DAOs
│   ├── services/          # NotificationService, AudioService, SchedulerService
│   ├── router/            # GoRouter configuration et routes
│   └── theme/             # Couleurs, typographie, thème global
├── features/
│   ├── onboarding/
│   │   ├── onboarding_screen.dart       # View
│   │   ├── onboarding_provider.dart     # ViewModel (Riverpod Notifier)
│   │   ├── onboarding_state.dart        # State immutable
│   │   └── widgets/
│   ├── home/
│   │   ├── home_screen.dart
│   │   ├── home_provider.dart
│   │   ├── home_state.dart
│   │   └── widgets/                     # WaterDropWidget, TimerWidget, etc.
│   ├── settings/
│   │   ├── settings_screen.dart
│   │   ├── settings_provider.dart
│   │   ├── settings_state.dart
│   │   └── widgets/
│   └── stats/
│       ├── stats_screen.dart
│       ├── stats_provider.dart
│       ├── stats_state.dart
│       └── widgets/
├── repositories/
│   ├── drink_repository.dart            # CRUD drink_logs
│   ├── user_repository.dart             # CRUD user_profile
│   └── reminder_repository.dart        # CRUD reminder_config
└── shared/
    ├── models/                          # DrinkLog, UserProfile, ReminderConfig
    └── widgets/                         # Composants réutilisables cross-features
```

### Modèles de données (SQLite)

**Table: user_profile**
```sql
- id (PRIMARY KEY)
- weight (REAL)
- activity_level (TEXT: sedentary, moderate, active, very_active)
- glass_volume_ml (INTEGER, default: 250)
- sleep_start (TEXT: HH:mm)
- sleep_end (TEXT: HH:mm)
- created_at (INTEGER: timestamp)
- updated_at (INTEGER: timestamp)
```

**Table: reminder_config**
```sql
- id (PRIMARY KEY)
- mode (TEXT: intelligent, preset, custom)
- preset_profile (TEXT: light, normal, intensive, null si custom)
- custom_interval_minutes (INTEGER, nullable)
- custom_time_slots (TEXT: JSON array, nullable)
- notification_sound_enabled (INTEGER: boolean)
- media_pause_enabled (INTEGER: boolean)
- created_at (INTEGER: timestamp)
- updated_at (INTEGER: timestamp)
```

**Table: drink_logs**
```sql
- id (PRIMARY KEY)
- scheduled_time (INTEGER: timestamp prévu)
- actual_time (INTEGER: timestamp réel, nullable si snooze/ignoré)
- status (TEXT: confirmed, snoozed, missed)
- response_time_seconds (INTEGER: temps entre notif et confirmation)
- estimated_volume_ml (INTEGER)
- created_at (INTEGER: timestamp)
```

**Table: achievements**
```sql
- id (PRIMARY KEY)
- achievement_key (TEXT: unique identifier du badge)
- unlocked_at (INTEGER: timestamp)
- progress (INTEGER: pour les badges progressifs, nullable)
```

---

## 🎨 Design UI/UX (Suggestions)

### Style général
- **Minimaliste et apaisant** (thème eau/hydratation)
- Palette de couleurs : Bleus/turquoises, blanc, touches de vert
- Animations fluides (60fps minimum pour la goutte)
- Mode sombre optionnel (bonus)

### Écran principal (Home)
- Grande goutte centrale (60-70% de l'écran)
- Timer en dessous (police claire, grande taille)
- Bouton "J'ai bu" en bas (visible mais pas intrusif quand pas nécessaire)
- Animation de la goutte qui respire légèrement même au repos

### Écran statistiques
- Graphiques en courbes (régularité sur le temps)
- Cartes de métriques (aujourd'hui, cette semaine, ce mois)
- Section badges en grille

### Écran paramètres
- Liste simple et claire
- Sections bien séparées : Profil, Rappels, Notifications, Avancé

---

## 🚧 Défis Techniques Identifiés

### 1. Contrôle audio cross-platform
- **iOS** : Très restrictif, nécessite Audio Session Framework
  - Peut détecter la lecture audio
  - Peut interrompre via audio session interruption
  - Limitations : Ne peut pas forcer la reprise de musique (décision de l'app tierce)

- **Android** : Plus permissif
  - AudioManager peut contrôler les médias
  - Media session APIs pour détecter et contrôler la lecture
  - Possibilité d'envoyer des commandes play/pause

**Solution proposée** :
- Android : Contrôle direct des médias via AudioManager (AudioFocus AUDIOFOCUS_GAIN_TRANSIENT)
- iOS : Utiliser l'audio session interruption (comportement natif)
  - Démarrer un audio silencieux pour interrompre les médias
  - Les apps tierces respecteront l'interruption
  - Reprise automatique impossible sur iOS (limitation Apple) ✅ *décidé*
  - **Solution iOS** : afficher un message explicatif lors de l'onboarding : "Sur iOS, relancez manuellement votre musique après avoir bu."
- Android : reprise automatique des médias via commande media PLAY après confirmation

### 2. Distinction des types de médias
- Difficile de différencier musique vs GPS vs appel au niveau système
- **Appels** : Détectables via phone_state (Android) et CallKit (iOS)
- **GPS/Navigation** : Pas de distinction claire au niveau audio

**Solution proposée** :
- Détecter les appels en cours → ne jamais interrompre
- Pour GPS : Liste blanche configurable d'apps à ne pas interrompre (optionnel, avancé)
- Par défaut : pause tous les médias sauf appels

### 3. Notifications en arrière-plan
- Scheduler des rappels précis même quand l'app est fermée
- **workmanager** pour Android (Doze mode, battery optimization)
- **Background Fetch** pour iOS (moins précis, limitations)

**Solution proposée** ✅ *décidé* :
- Scheduler toute la journée de rappels au démarrage de l'app
- Re-scheduler chaque nuit à minuit via background task (workmanager)
- Sur Android : écouter BOOT_COMPLETED pour reprogrammer après redémarrage du téléphone
- Payload de chaque notification : `{"type": "reminder", "reminder_id": <id>}` pour routing GoRouter
- Demander à l'utilisateur de désactiver les optimisations de batterie (Android)

---

## 📝 Notes Additionnelles

### Hors scope du MVP (futures versions)
- Intégration météo en temps réel
- Synchronisation cloud / multi-devices
- Widget home screen
- Apple Watch / Wear OS
- Partage de statistiques / compétition entre amis
- Rappels basés sur l'activité physique (accéléromètre)
- Intégration Apple Health / Google Fit

### Considérations légales/UX
- **Permissions requises** : Notifications, contrôle audio, (appels pour détection)
- Expliquer clairement pourquoi chaque permission est nécessaire
- Option pour désactiver la pause des médias si trop contraignant
- ~~Mode vacances / pause de l'app~~ → hors scope, feature non retenue

### Tests prioritaires
- Test de la pause audio sur différentes apps (Spotify, YouTube, Apple Music, etc.)
- Test en arrière-plan sur différents devices (battery saver, Doze mode)
- Test des notifications sur différentes versions Android/iOS
- Test de l'animation de la goutte (performance, 60fps)

---

## 🎯 Prochaines Étapes

1. **Validation du design** par l'utilisateur
2. **Plan d'implémentation détaillé** (via superpowers:writing-plans)
3. **Setup du projet** :
   - Architecture des dossiers
   - Initialisation SQLite
   - Setup des services de base
4. **Développement par features** :
   - Phase 1 : Goutte animée + timer + stockage local
   - Phase 2 : Système de notifications
   - Phase 3 : Contrôle audio/médias
   - Phase 4 : Onboarding
   - Phase 5 : Statistiques et badges
   - Phase 6 : Polish et tests

---

## 🔲 Points encore à explorer (session suivante)

### UI/UX technique
- [ ] Animation de la goutte : CustomPainter (Flutter pur) ou Rive ?
- [ ] Mode sombre : dans le MVP ou v2 ?
- [ ] Design system : typographie, spacing, composants réutilisables
- [ ] Onboarding flow détaillé : transitions, validation des champs, skip possible ?
- [ ] App icon & splash screen

### Non-fonctionnel
- [ ] Accessibilité : support screen readers, tailles de police dynamiques
- [ ] Localisation/i18n : stratégie (flutter_localizations + arb files ?)
- [ ] Stratégie de tests : unit / widget / integration, couverture minimale
- [ ] Migration SQLite : stratégie pour les futures versions de la DB

---

**Document de brainstorming créé le 2026-01-08**
**Mis à jour le 2026-03-01 — décisions architecture, notifications, audio, fonctionnalités**
**Session de brainstorming en cours — points UI/UX et non-fonctionnel restants**
