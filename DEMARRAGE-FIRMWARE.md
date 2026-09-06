# Démarrage — bascule vers le firmware

But : passer de l'outil PC (Python, aujourd'hui gelé sur `main`) au fork de
firmware Momentum, en éprouvant la méthode sur **une seule** fonctionnalité
avant de s'engager sur les six piliers. Trois agents participent désormais —
Opus, Gemini, et l'agent de code Copilot (voir la « Répartition du travail »
d'`AGENTS.md`).

Ce fichier décrit l'ordre du démarrage. La vision et l'ordre des chantiers
restent dans `FEUILLE-DE-ROUTE.md`.

## Prérequis — avant la première ligne de C

1. **Trancher le sort de `main`.** La charte firmware vit pour l'instant sur
   une branche ; `main` reste l'outil Python cohérent. Décider : le firmware
   devient-il `main` (avec archivage de l'outil Python sous l'étiquette
   `archive/outil-python`), ou vit-il sur une branche ou un dépôt dédié ? Rien
   ne se pousse tant que ce n'est pas tranché.
2. **Pousser le dépôt sur GitHub.** Indispensable, et pas seulement confortable :
   l'agent Copilot travaille par **issue → pull request** sur github.com. Sur un
   dépôt purement local, il ne peut pas participer du tout. Aucun distant n'est
   configuré à ce jour — c'est le premier geste qui débloque le troisième agent.
3. **Forker Momentum-Firmware** (`Next-Flip/Momentum-Firmware`). Décider tôt :
   fork complet piloté par `fbt`, ou applications autonomes `ufbt` (FAP) ?
   Recommandation : **commencer en `ufbt`**. Une FAP se construit, se flashe et
   s'itère sans reconstruire tout le firmware — c'est le cycle le plus court
   pour éprouver la méthode avant de s'engager sur un fork complet.
4. **Monter la toolchain.** `ufbt` gère le SDK et la toolchain ARM. Valider
   qu'une build de référence passe avant d'écrire du code produit.

## Étape 1 — Boucle matérielle validée (« hello FAP »)

Prouver le cycle build → flash → exécution sur l'appareil réel, rien de plus.

- Livrable : une FAP minimale qui s'affiche à l'écran du Flipper.
- Le flash est **une action humaine** (`AGENTS.md`) : l'agent prépare le
  binaire, l'humain le pose sur l'appareil.
- Critère de sortie : la FAP tourne sur le matériel et l'équipe sait reproduire
  le cycle de bout en bout.

## Étape 2 — Premier pilier prototypé en FAP

Choisir **un** pilier réaliste en FAP autonome, avant tout fork complet.

- Candidat recommandé : **gestionnaire de modules** (pilier 5). Périmètre net
  (détecter une carte externe, présenter son profil), plus borné qu'un clavier
  système, et il rejoue en C un besoin que l'outil Python a déjà défriché côté
  PC — le besoin est donc compris, seule la mise en œuvre embarquée est neuve.
- Alternative : le **clavier prédictif** (pilier 3, phare). Plus d'impact, mais
  il touche à l'entrée système — moins adapté à un premier prototype isolé.
- Livrable : une fiche Opus complète (contrat, périmètre, **budget flash/RAM
  annoncé**, critères d'acceptation mesurables), puis l'implémentation.

## Étape 3 — Roder la boucle à trois agents

Sur ce premier pilier, éprouver le circuit complet, une fois :

1. **Opus** rédige la fiche (contrat + budget flash/RAM + critères mesurables).
2. La fiche devient une **issue GitHub**.
3. Aiguillage (`AGENTS.md`) : jugement → Opus ; unité bornée livrée en PR →
   Copilot ; large et peu profond → Gemini.
4. L'exécutant livre — une **pull request** pour Copilot.
5. **Revue `reviseur`** (Opus, contexte séparé, sans droit d'écriture) — y
   compris sur une PR Copilot. Aucune fusion sans verdict.
6. Fusion, budget flash/RAM vérifié à la fusion.

C'est cette boucle, et non la première fonctionnalité elle-même, qui est le
vrai livrable de l'étape : tant qu'elle n'est pas fluide, on n'élargit pas.

## Étape 4 — Élargir aux six piliers

Une fois la boucle rodée, dérouler la feuille de route dans l'ordre P0 → P1
(`FEUILLE-DE-ROUTE.md`), une fiche par chantier, chacune avec son budget
mémoire et son moyen de mesure.

## Ce qui ne change pas

Les garde-fous d'`AGENTS.md` valent dès la première FAP : flasher est une
action humaine, un seul outil parle au Flipper à la fois, jamais de suppression
sur la carte SD, profils régionaux conservateurs par défaut, et aucun garde-fou
désactivé pour faire passer un test.
