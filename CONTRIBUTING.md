
# CONTRIBUTING.md — Guide de contribution

> Comment contribuer à Kōeki Claude Skills B2B FR. Bienvenue à toutes les contributions qui respectent les principes du repo.

---

## Avant de commencer

Lis dans cet ordre :
1. [README.md](./README.md) — pour comprendre la vision du repo
2. [ARCHITECTURE.md](./ARCHITECTURE.md) — pour comprendre les 4 couches et le pattern Coquille/Hub
3. [shell/SHELL.md](./shell/SHELL.md) — pour comprendre la voix éditoriale Kōeki
4. [agents/agent-socratic-mode.md](./agents/agent-socratic-mode.md) — pour comprendre la maïeutique, notre signature

Si tu as lu ces 4 documents et que tu adhères aux principes, tu es prêt à contribuer.

---

## Ce qu'on accueille avec joie

### Nouvelles skills B2B
- Skills pour secteurs francophones spécifiques (santé, défense, aéronautique, énergie, agriculture)
- Skills pour outils B2B non encore couverts (Pipedrive, Brevo, ActiveCampaign, Mautic, Pardot)
- Skills pour étapes du cycle de vente non couvertes (qualification, closing, onboarding client)

### Nouvelles skills socratiques pures
Ces skills ne produisent rien, elles font penser. C'est notre signature. Si tu as une méthode de réflexion structurée qui mérite d'être systématisée, on est preneurs.

### Améliorations de skills existantes
- Ajouts de cas particuliers sectoriels
- Anti-patterns spécifiques identifiés sur le terrain
- Cas d'usage industriels (NDA, marchés publics, ITAR/EAR)

### Nouveaux playbooks d'orchestration
Séquences de prompts pour les non-techniciens qui ne savent pas piloter Claude librement. Particulièrement utiles pour les équipes commerciales/marketing PME.

### Références méthodologiques
Documents denses sur des frameworks éprouvés (Ulwick JTBD, Challenger Sale, MEDDIC, etc.) adaptés à un contexte français.

### Traductions
Vers d'autres francophonies : Québec, Belgique, Suisse, Afrique. Les nuances culturelles et réglementaires comptent.

### Cas d'usage réels documentés
Si tu utilises une skill en production et que tu veux partager ce qui a marché (anonymisé), c'est précieux pour la communauté.

---

## Ce qu'on n'accueille PAS

- Skills B2C, e-commerce généraliste, crypto, dropshipping
- Skills qui contournent la maïeutique (production déléguée pure, sans mode socratique)
- Skills qui utilisent des biais cognitifs interdits (fausse urgence, fausse rareté, manipulation émotionnelle)
- Skills calquées sur des skills anglophones existantes sans valeur ajoutée francophone B2B
- Skills qui ignorent le RGPD ou les contraintes réglementaires européennes
- Contributions générées intégralement par IA sans relecture humaine experte
- Skills qui ne respectent pas la voix éditoriale Kōeki (test du café raté)

---

## Process de contribution

### 1. Ouvrir une issue de proposition

Avant d'écrire du code/contenu, ouvre une issue avec :
- **Titre** : type[scope] description (ex : `feat[skills] : nouvelle skill cold-email-sante-medical`)
- **Description** :
  - Le job en une phrase
  - Pour qui c'est utile et pourquoi
  - Pourquoi ça n'existe pas déjà dans le repo
  - Si ça reprend une méthode existante, attribution explicite

On discute le périmètre en commentaires avant que tu ne te lances.

### 2. Fork et branch

```bash
gh repo fork sebastiengrillot/koeki-claude-skills-b2b-fr --clone
cd koeki-claude-skills-b2b-fr
git checkout -b skill/{nom-court-skill}
```

### 3. Produire en suivant les agents

Si tu utilises Claude Code dans le repo cloné :
1. Lis `CLAUDE.md` (CC le lit auto)
2. Lis `agents/agent-skill-writer.md` et applique sa méthode en 7 étapes
3. Lis `agents/agent-skill-tester.md` et génère tes evals
4. Lis `agents/agent-skill-reviewer.md` et audite ta propre production

Si tu produis manuellement, applique les mêmes principes :
- Frontmatter YAML correct (name + description "pushy")
- Référence à la Coquille (pas de re-description)
- Mode socratique intégré (5-7 questions)
- Anti-patterns spécifiques (pas génériques)
- Format de sortie standardisé
- Question d'amorce

### 4. Tester

Tu lances tes evals AVEC et SANS ta skill. Tu compares qualitativement et quantitativement. Tu écris les résultats dans `evals/{skill-name}/evals.json`.

Pas de PR sans evals. C'est non-négociable.

### 5. Soumettre la PR

```bash
git push origin skill/{nom-court-skill}
gh pr create --base dev --title "feat(skills): ajout {nom-skill}" --body-file PR_BODY.md
```

Le PR_BODY.md doit contenir :
- Le résumé en 3 lignes de ce que la skill apporte
- Le lien vers l'issue de proposition
- Le rapport d'eval (résultats AVEC/SANS la skill)
- La note sur "pourquoi cette skill respecte les principes du repo"
- Tout cas particulier à valider (RGPD, NDA, secteur sensible)

### 6. Revue

Sébastien (ou un mainteneur désigné) review ta PR avec `agent-skill-reviewer.md`. Possible retours :
- ✅ MERGE direct
- 🔧 MODIFS demandées (avec commentaires précis)
- ⏸️ DISCUSSION nécessaire (si question de périmètre / philosophie)
- ❌ REJETÉE (si non-respect des principes, après discussion)

### 7. Merge

Si MERGE accepté, ta PR atterrit dans `dev`. Périodiquement, `dev` est mergé dans `main` avec un tag de version.

Tu apparaîs dans les contributeurs du repo. Mention dans le CHANGELOG.

---

## Conventions de code/contenu

### Naming
- Skills : `b2b-*`, `hubspot-*`, `lemlist-*`, `socratic-*` selon le domaine, toujours kebab-case
- Méta-prompts : `meta-prompt-{outil}-skill-creator.md`
- Playbooks : `playbook-{action}.md`
- References : `reference-{sujet}.md`

### Format Markdown
- Headers H1 unique en haut du document
- Headers H2 pour les sections principales
- Listes numérotées pour les séquences (workflow, étapes)
- Listes à puces pour les listes non-séquentielles
- Tableaux pour les comparaisons / matrices
- Citations `> ...` pour les exemples de prompts ou de sorties
- Code blocks ` ``` ` pour les YAML, JSON, snippets

### Voix
Toute contribution applique la voix Kōeki définie dans `shell/SHELL.md`. Si tu n'es pas sûr de la voix, écris d'abord en mode "neutre professionnel" et on adaptera ensemble en revue. Ne pas inventer une voix qui ne te correspond pas — c'est mieux qu'on harmonise après.

### Attribution
Si ta skill s'inspire d'un framework existant (méthode Ulwick, frameworks Lemlist, etc.), tu cites la source. Pas de plagiat même involontaire.

---

## Charte éthique de contribution

En contribuant, tu t'engages à :

1. **Ne pas produire de skills manipulatoires.** La frontière biais éthique / manipulation est définie dans `shell/SHELL.md`. Tu la respectes.

2. **Préserver le mode socratique.** Toute nouvelle skill propose le mode socratique. C'est notre signature, pas une option.

3. **Respecter le RGPD.** Toute skill qui touche aux données prospects/clients respecte les garde-fous universels.

4. **Anonymiser les exemples.** Pas de nom réel d'entreprise, de personne, ou d'information confidentielle dans tes exemples.

5. **Documenter tes sources.** Si tu reprends une méthode tierce, tu cites.

6. **Accepter la revue critique.** Le repo a un standard de qualité. Si une PR est demandée pour modifications, tu reçois les retours sans défensive.

---

## Sur l'usage d'IA pour contribuer

L'IA (Claude, ChatGPT, autres) est bienvenue comme outil de production. Mais :
- Tu relis tout ce qui sort de l'IA avant de soumettre. Pas de copier-coller brut.
- Tu testes empiriquement, pas seulement à l'œil.
- Tu apportes ton expertise métier — l'IA n'a pas la nuance terrain.
- Une contribution générée 100% par IA sans relecture experte sera rejetée.

L'objectif : qualité humaine augmentée, pas production industrielle automatisée.

---

## Reconnaissance des contributeurs

Tu apparais dans :
- Le fichier `CONTRIBUTORS.md` (alphabétique)
- Le `CHANGELOG.md` pour les releases qui incluent ta contribution
- Le frontmatter `metadata.author` de la skill que tu as produite (si tu le souhaites)

Pour les contributions majeures (5+ skills produites, ou maintenance long terme), tu peux être proposé comme co-mainteneur du repo.

---

## Code de conduite

Le repo applique le [Contributor Covenant](https://www.contributor-covenant.org/) v2.1. Comportements attendus :
- Bienveillance et respect
- Critique constructive
- Patience envers les contributeurs débutants
- Reconnaissance du travail des autres

Comportements interdits :
- Harcèlement, insultes, discrimination
- Trolling, attaques personnelles
- Spam ou auto-promotion non liée
- Partage d'infos privées sans consentement

Les violations sont à signaler à [sebastien@koeki.fr](mailto:sebastien@koeki.fr).

---

## Questions

- Bug ou problème technique : ouvre une issue sur GitHub
- Question philosophique sur le repo : ouvre une discussion GitHub
- Contact direct : [sebastien@koeki.fr](mailto:sebastien@koeki.fr) ou [LinkedIn](https://www.linkedin.com/in/sebastiengrillot)

---

**Merci de contribuer à Kōeki Claude Skills B2B FR.**

*Made with ❤️ — Kōeki Agency, Tarascon, France.*
