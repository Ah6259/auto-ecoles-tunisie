# Mémoire du projet — annuaire (moteur commun des annuaires d'Ahmed)

Fichier lu par Claude Code au début de chaque session. **Dépôt PUBLIC : rien de personnel ni de secret, jamais le nom d'un concurrent.**
Répondre à Ahmed **en français**, simplement. Règles communes : `../../regles communes a tous les sites.md`.

## Principe
- Un **moteur commun** (`annuaires/moteur/`, sur le PC d'Ahmed) copié dans chaque annuaire par `python annuaires/synchroniser.py`.
  **Ne jamais modifier le moteur directement dans un site** : modifier `annuaires/moteur/`, synchroniser, puis tester chaque site.
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
- `inscriptions_ouvertes: false` tant que la **déclaration INPDP** n'est pas faite (loi organique 2004-63). Les demandes
  d'ajout / correction / retrait restent possibles. Un retrait est définitif (`donnees/retraits.json`).
- Fiche gratuite pour tous ; fiche **Pro** payante plus tard (champ `pro: true`, mise en avant), découverte au moment du besoin.
- Statistiques anonymes GoatCounter (compteur prix-eaux-tunisie) : `clic-tel|whatsapp|itineraire/<fiche>` = argument de vente Pro.

## Robots (sans PC)
- `maj.yml` chaque nuit (02h40 UTC) : relevé OSM (`tools/releve_osm.py` : plusieurs serveurs, nouvelles tentatives, refus si
  chute de plus de 50 % des fiches), pages, tests, publication. `tests.yml` à chaque modification. Échec → e-mail GitHub.

## Tests
`node tools/construire.mjs` puis `node tools/test_site.mjs` → **TOUT PASSE** (jsdom : `npm install --no-save --no-package-lock jsdom`).

## Règles du moteur (mise à jour du 06/10/2026, détail dans annuaires/moteur/CLAUDE.md)
- **Aucune fiche vide** : une fiche sans téléphone, WhatsApp, site ni page publique n'est pas publiée (règle d'Ahmed, pour le sérieux).
- Au moins 3 fiches par métier et par gouvernorat ; sources affichées telles quelles (osm, web = page de l'établissement, officiel = liste d'une administration).
- Vraie photo par métier (config.metiers[].photo) ; espace professionnels gratuit + Pro (1er mois offert), fermé jusqu'à l'INPDP.
