# Mémoire du projet — annuaire (moteur commun des annuaires d'Ahmed)

Fichier lu par Claude Code au début de chaque session. **Dépôt PUBLIC : rien de personnel ni de secret, jamais le nom d'un concurrent.**
Répondre à Ahmed **en français**, simplement. Règles communes : `../../regles communes a tous les sites.md`.

## Principe
- Un **moteur commun** (`annuaires/moteur/`, sur le PC d'Ahmed) copié dans chaque annuaire par `python annuaires/synchroniser.py`.
  **Ne jamais modifier le moteur directement dans un site** : modifier `annuaires/moteur/`, synchroniser, puis tester chaque site.
  Exception : une session SANS le PC (téléphone, Claude en ligne) corrige directement les sites et le note ici ; le PC reporte
  ensuite dans le moteur. Depuis le 08/10/2026, `synchroniser.py` REFUSE de copier si un site a changé depuis la dernière
  synchronisation (empreintes `annuaires/.synchro/`) : faire `git pull`, reporter dans moteur/, puis relancer (`--force` après vérification).
- Propre à chaque site : `config.json` (nom, couleurs, métiers et étiquettes OpenStreetMap, liens vers nos autres sites),
  `donnees/` (osm.json = robot ; inscrits.json = fiches vérifiées ; retraits.json = fiches retirées, jamais republiées),
  `assets/logo.svg`, `assets/icons/` (famille d'icônes commune : `annuaires/icones_annuaires.py`).
- Pages fabriquées par `node tools/construire.mjs` (ne pas les modifier à la main) : accueil (recherche + filtres),
  24 gouvernorats, une page par fiche (JSON-LD), Professionnels (ajout / correction / retrait par Formspree), À propos.

## Checklist visuelle (obligatoire, bloquée par le test)
- **Photo du bandeau** : `python annuaires/photo_commons.py <id> "File:…" "alt FR" "alt AR"` (Wikimedia, licence libre, preuve
  sauvegardée dans `annuaires/preuves/`, crédit affiché ; pas de visage reconnaissable, pas d'emblème de l'État).
- **Image en couleur par métier** : `assets/metiers/<id-métier>.svg` (48 × 48, fond pastel, aplats, accent doré).
- **Image d'aperçu** : `python annuaires/apercu.py <id>` → `assets/og-image-vN.jpg` (nouveau nom à chaque fois).
- Icône : `python annuaires/icones_annuaires.py <id>` (+ `icone.json`).

## Données et loi
- Sources permises : **OpenStreetMap** (licence ODbL, crédit sur chaque page), demandes des professionnels. **Jamais** de copie
  d'un annuaire concurrent, du RNE ou de Google Maps.
- `inscriptions_ouvertes: true` depuis le 06/10/2026 (accord de l'INPDP obtenu par Ahmed, `config.pro.inpdp_accord` ; loi organique 2004-63). Les demandes
  d'ajout / correction / retrait restent possibles. Un retrait est définitif (`donnees/retraits.json`).
- Fiche gratuite pour tous ; fiche **Pro** payante plus tard (champ `pro: true`, mise en avant), découverte au moment du besoin.
- Statistiques anonymes GoatCounter (compteur prix-eaux-tunisie) : `clic-tel|whatsapp|itineraire/<fiche>` = argument de vente Pro.

## Robots (sans PC)
- `maj.yml` le 1er de chaque mois (02h40 UTC, décision d'Ahmed du 10/10/2026 ; aussi à chaque modification de `config.json` et à la main) : relevé OSM (`tools/releve_osm.py` : plusieurs serveurs, nouvelles tentatives, refus si
  chute de plus de 50 % des fiches), pages, tests, publication. `tests.yml` à chaque modification. Échec → e-mail GitHub.
- **18/10/2026 (VS.8, règle commune du 15/10)** : `maj.yml` ENREGISTRE (commit + push avec 5 essais) `donnees/osm.json` (+ `importes.json`) juste après le relevé, AVANT les pages et les tests ; les pages ne sont publiées que si les tests passent. Tout dans le même robot (un seul robot en attente par groupe de concurrence). Le `maj.yml` n'est PAS copié par `synchroniser.py` : le même fichier est dans les 5 annuaires, à modifier partout pareil.

## Tests
`node tools/construire.mjs` puis `node tools/test_site.mjs` → **TOUT PASSE** (jsdom : `npm install --no-save --no-package-lock jsdom`).

## Règles du moteur (mise à jour du 06/10/2026, détail dans annuaires/moteur/CLAUDE.md)
- **Aucune fiche vide** : une fiche sans téléphone, WhatsApp, site ni page publique n'est pas publiée (règle d'Ahmed, pour le sérieux).
- Au moins 3 fiches par métier et par gouvernorat ; sources affichées telles quelles (osm, web = page de l'établissement, officiel = liste d'une administration).
- Vraie photo par métier (config.metiers[].photo) ; espace professionnels gratuit + Pro (1er mois offert), fermé jusqu'à l'INPDP.

- **Vidéos de présentation** (06/10/2026) : `assets/video/presentation.mp4` (visiteurs) et `presentation-pro.mp4` (professionnels), 1080 × 1920, + couvertures et aperçus 1200 × 630 (`apercu-video*.jpg`). Musique de fond : J. S. Bach, Aria des Variations Goldberg (enregistrement Musopen, CC0, Wikimedia Commons ; preuve dans le dossier privé `videos (outil)/preuves musique/`). Pages **`video/`** et **`video-pro/`** fabriquées par `tools/page_video.mjs` (appelé par construire.mjs, à partir de À propos : mêmes en-tête, pied, CSP) avec og:video / og:image. Le bouton « Partager » envoie un LIEN : la page vidéo (video-pro/ sur inscription/) + l'adresse du site dans le texte, jamais le fichier. Tests dans test_site.mjs.
  Pour refaire les vidéos : `python fabriquer.py <id-du-site>` dans le dossier PRIVÉ du PC `videos (outil)/`.

- **Adresse depuis le 11/10/2026 : https://auto-ecoles.clicvia.com/** (config.json « url » ; le moteur écrit le fichier CNAME, préfixe GoatCounter /auto-ecoles-tunisie). Ligne DNS `CNAME auto-ecoles → ah6259.github.io` nuage gris. L'ancienne adresse ah6259.github.io/auto-ecoles-tunisie/ redirige seule.

- **Visiteur simulé (18/10/2026, règle générale d'Ahmed : signaler les problèmes AVANT le visiteur réel)** : `python3 tools/visiteur_simule.py`
  (outil COMMUN à tous les sites, copie identique, ne pas le modifier ici) — liens/images internes morts, puis Chrome à 390 px : page plus
  large que l'écran, image cassée, « NaN / undefined », page vide. Scénarios `tools/scenarios-visiteur.json` FABRIQUÉS par construire.mjs
  à partir des vraies fiches : recherche par nom, par gouvernorat, par métier (un filtre de plus ne donne jamais plus de résultats), page
  d'un gouvernorat, fiche avec son bouton « Appeler », accueil en arabe. Lancé par `tests.yml` et `maj.yml` (étape « Visiteur simule »).
  1re anomalie trouvée : en-tête trop large sur téléphone (Mariage 395 px, Professionnels 417 px) → corrigé dans style.css du moteur.

- **Lieux officiels (18/10/2026, règle commune § 11.11)** : test_site.mjs vérifie dans les deux sens que la liste « Gouvernorat » (accueil, formulaires, pages, carte) = les 24 gouvernorats de La Poste, rien d'autre (les annuaires ne filtrent pas par délégation ; « ville » = texte libre) ; la carte de l'accueil entoure le gouvernorat choisi dans le filtre (annuaire.js).
- 18/10/2026 : recherche « au son » (Jerba = Djerba, Houmt = Houmet, Qairouan = Kairouan, lettres doublées…), même calcul pour ce qu'on tape et pour la liste ; test de paires d'écritures.

- **19/10/2026 — Visiteur simulé : contrôle des ESPACES (§ 29)** : cases gouvernorat / métier, le nombre était COLLÉ au nom (« Traducteurs66 ») ou SORTAIT de la case (« Photographes 46 ») → corrigé dans style.css du moteur (le nombre passe sous un nom trop long) ; 0 anomalie.
