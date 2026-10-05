# ⚠️ À LIRE AVANT DE TOUCHER AU PROGRAMME (DpixM/cnc)

Ce soft (`index.html`, en ligne sur https://dpixm.github.io/cnc/) est modifié par
**plusieurs conversations à la fois** : le dessin / la CAO d'un côté, l'onglet **CIP Trumpf**
de l'autre. Pour ne **RIEN casser** et ne **RIEN perdre**, toute conversation (humaine ou IA)
qui touche au programme **DOIT** suivre ces règles.

## Les 5 règles

1. **Avant de modifier : repartir de la DERNIÈRE version de `main`.**
   `git fetch origin main` puis partir de `origin/main`. **Jamais** d'une vieille copie —
   sinon tu écrases le travail déjà en ligne d'une autre conversation.

2. **Ne toucher QUE sa zone.**
   - Dessin / CAO / G-code / laser = « zone dessin ».
   - Onglet CIP = **uniquement** entre les marqueurs « ZONE CIP TRUMPF » dans `index.html`
     (HTML `#cipBody` + bloc JS `cip_*` / `cipState`). Rien en dehors de ces marqueurs.

3. **Ne JAMAIS pousser sur `main` sans que Seb ait TESTÉ et VALIDÉ.**
   Méthode : branche de test → lien `raw.githack.com/DpixM/cnc/<branche>/index.html` →
   Seb teste et valide → **ensuite seulement** `main`.

4. **Après validation : pousser sur `main`.**
   Si le push est **refusé** (non fast-forward), c'est que `main` a bougé entre-temps :
   refaire l'étape 1 (re-synchro) puis repousser. **Ne jamais forcer** un push qui
   écraserait le travail d'une autre conversation.

5. **Résultat :** en repartant toujours d'un `main` à jour et en restant dans sa zone,
   les deux conversations restent **synchro automatiquement** (`main` = seule source de vérité).

## Filets de sécurité

Deux branches de sauvegarde figées, à restaurer en cas de pépin :
- `sauvegarde-soft-2026-10-01` — le soft avant la fusion CIP + améliorations.
- `sauvegarde-main-cip-2026-10-03` — le soft juste avant la fusion (avec le CIP).

Et de toute façon : **GitHub garde toutes les versions** — rien n'est jamais vraiment perdu.

## Rappels techniques (déploiement depuis le PC de Seb)

- Push cloud bloqué (403 proxy) → on pousse depuis le PC de Seb, avec son token
  (jamais écrit dans un fichier ; en ligne dans la commande, logs nettoyés `sed 's/ghp_[A-Za-z0-9]*/ghp_***/g'`).
- Transfert cloud→PC du gros `index.html` : via base64 (`base64 -w0` → fichier `.b64` →
  décodage sur le PC `base64 -d`), avec vérif `sha256sum` identique des deux côtés avant push.
- Le badge de version du soft est en haut à droite (`vAAAA-MM-JJ-XX`). L'onglet CIP a son propre
  numéro de version à lui.

**En deux mots : repars du dernier `main`, reste dans ta zone, fais valider par Seb, puis pousse.**
