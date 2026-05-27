# Guide Cue Creator

Lit une session Ableton Live (`.als`), associe les repères aux fichiers de cue audio et génère un fichier WAV stéréo unique (`guide_cues.wav`) prêt à être déposé sur une piste de cue guide dédiée au temps 0.

---

## Contenu inclus

```
cue_creator.html    ← l'application (ouvrir ce fichier)
GUIDE CUES/         ← bibliothèque de cues audio (préchargée)
```

---

## Configuration requise

- **Chrome ou Edge** (recommandé) — accès au dossier sans clic supplémentaire après la première utilisation
- **Firefox** — fonctionne, sélectionner à nouveau le dossier une fois par session de navigateur si nécessaire
- Connexion Internet (charge pako, Fuse.js, Dexie depuis CDN)

---

## Démarrage rapide

1. Double-cliquez sur `cue_creator.html` — s'ouvre dans votre navigateur par défaut
2. Cliquez sur **Sélectionner un dossier** et choisissez le dossier contenant `GUIDE CUES/`
3. Sélectionnez une **langue** dans le menu déroulant
4. Faites glisser votre fichier `.als` sur la zone de téléchargement (ou cliquez pour parcourir)
5. Vérifiez le tableau des cues — vert = bonne correspondance, jaune = faible confiance, rouge = aucune correspondance
6. Cliquez sur **Aperçu** (ou appuyez sur **Espace**) pour écouter
7. Cliquez sur **Rendre en WAV** — télécharge `guide_cues.wav`
8. Faites glisser `guide_cues.wav` dans votre session Ableton au temps 0 sur une piste dédiée

Sur Chrome/Edge, le choix du dossier est mémorisé. L'étape 2 est une configuration unique.

---

## Ce que fait l'application

- Place chaque cue de section **1 mesure avant** son repère
- Génère un **compte à rebours** pour les 2 premières mesures (temps 0–7 en 4/4)
- Supprime les cues de section qui tombent dans la région du compte à rebours
- Ignore les repères nommés `count off` ou `next song`
- Prend en charge l'automatisation du tempo (sessions à BPM variable)

---

## Signatures rythmiques prises en charge

3/4 · 4/4 · 6/8 · 12/8

---

## Langues prises en charge

Anglais · Espagnol · Français · Indonésien · Coréen · Filipino · Portugais

---

## Contrôles du tableau des cues

| Colonne | Action |
|---------|--------|
| Repère | Lecture seule — depuis votre .als |
| Cue associé | Modifier pour remplacer la correspondance automatique |
| Fiabilité | Vert ≥ 80% · Jaune ≥ 50% · Rouge < 50% |

---

## Contrôles de la timeline

| Action | Résultat |
|--------|----------|
| Clic | Aller à la position |
| Espace | Lecture / Pause |
| Ctrl+défilement ou pincement | Zoom vers le curseur |
| Boutons +/− | Zoom avant/arrière |
| Réinitialiser | Adapter la session à la largeur |
| Suivre la lecture | Activer/désactiver le défilement automatique |

---

## Caractéristiques de sortie

| Paramètre | Valeur |
|-----------|--------|
| Fréquence d'échantillonnage | 48 000 Hz |
| Canaux | Stéréo |
| Profondeur de bits | PCM 16 bits |
| Fichier | `guide_cues.wav` |
