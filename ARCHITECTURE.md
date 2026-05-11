
# Architecture — Kōeki Claude Skills B2B FR

> Doc d'architecture du repo. Explique pourquoi les 4 couches existent, comment elles s'enrichissent l'une l'autre, comment elles héritent de la Coquille, et où vit chaque type de contenu.

---

## Vue d'ensemble : l'architecture en 4 couches

Plutôt qu'une bibliothèque plate de skills (modèle Tom Granot ou Lemlist), ce repo organise son contenu en 4 couches qui s'enrichissent l'une l'autre.

```
┌─────────────────────────────────────────────────────────────────┐
│              COUCHE 4 — RÉFÉRENCES MÉTHODOLOGIQUES              │
│  Matière brute, frameworks documentés, méthodes éprouvées.      │
│  Contexte permanent pour les Projects Claude.                   │
│  Ex : reference-jtbd-strategyn.md, reference-rgpd-b2b-fr.md     │
└────────────────────────────┬────────────────────────────────────┘
                             │  alimente
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│           COUCHE 3 — PLAYBOOKS D'ORCHESTRATION                  │
│  Séquences de prompts qui orchestrent l'usage des skills.       │
│  Pour les non-techniciens qui veulent un parcours guidé.        │
│  Ex : playbook-setup-crm-outreach.md                            │
└────────────────────────────┬────────────────────────────────────┘
                             │  invoque
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│        COUCHE 2 — MÉTA-PROMPTS DE PERSONNALISATION              │
│  Prompts qui génèrent des skills custom par entreprise          │
│  (interview + lecture MCP → skill calibrée).                    │
│  Ex : meta-prompt-hubspot-skill-creator.md                      │
└────────────────────────────┬────────────────────────────────────┘
                             │  customise
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│         COUCHE 1 — SKILLS D'EXPERTISE B2B GÉNÉRIQUE             │
│  Skills réutilisables, calibrées sur le standard Agent Skills.  │
│  Une skill = un job. Mode socratique intégré.                   │
│  Ex : b2b-personas-jtbd, b2b-cold-copy-psy, hubspot-pipeline-b2b│
└─────────────────────────────────────────────────────────────────┘
                             ▲
                             │  hérite de
                             │
┌─────────────────────────────────────────────────────────────────┐
│               🐚 LA COQUILLE (shell/SHELL.md)                    │
│  Voix éditoriale + anti-patterns universels + garde-fous RGPD   │
│  + format de sortie standardisé + frontière éthique cognitive.  │
│  Transversal à TOUS les éléments du repo.                       │
└─────────────────────────────────────────────────────────────────┘
```

---

## Pourquoi 4 couches plutôt qu'une bibliothèque plate

### Le problème des bibliothèques plates

Quand tu as 30+ skills à plat (Tom Granot, Lemlist, OpenClaudia), tu obtiens :
- **Duplication** : la voix, les anti-patterns, les garde-fous RGPD sont répétés dans chaque skill
- **Incohérence** : avec le temps, certaines skills divergent — voix qui change, anti-patterns différents
- **Maintenance lourde** : changer la voix nécessite de modifier 30 fichiers
- **Découverte difficile** : l'utilisateur ne sait pas quelle skill utiliser quand ni dans quel ordre

### Notre solution : héritage par couches

Chaque couche s'enrichit de celles du dessous, et hérite de la Coquille.

- Une **skill** ne contient QUE ce qui lui est spécifique. La voix, les anti-patterns universels, le RGPD viennent de la Coquille.
- Un **méta-prompt** ne contient QUE la logique de personnalisation. Les skills à générer suivent automatiquement le standard.
- Un **playbook** orchestre des skills existantes. Pas de répétition de contenu.
- Une **référence** est de la matière documentaire que les couches supérieures peuvent invoquer.

Résultat : changer la voix se fait dans 1 fichier (la Coquille). Les 7 skills, 2 méta-prompts, 2 playbooks et N références héritent automatiquement.

---

## Détail des 4 couches

### Couche 1 — Skills d'expertise B2B générique

**Emplacement** : `skills/{nom}/SKILL.md`

**Caractéristiques** :
- Frontmatter YAML obligatoire (name + description "pushy")
- Une skill = un job (règle Anthropic)
- Mode socratique activable (cf. agents/agent-socratic-mode.md)
- Maximum 500 lignes, sinon scinder en `skills/{nom}/references/`
- Test cases dans `evals/{nom}/evals.json`

**Quand créer une nouvelle skill** :
- La tâche est récurrente (≥ 1 fois / semaine dans la vie réelle)
- Il y a des décisions complexes que Claude ne prend pas bien par défaut
- Il y a de l'expertise métier accumulée à compresser
- Le job est étroit et nommable en une phrase

**Quand NE PAS créer une skill** :
- La tâche est unique ou rare → prompt one-off
- La tâche est triviale → Claude la fait bien sans skill
- Le sujet est trop large → découper en plusieurs skills atomiques
- Tu ne sais pas exprimer le job en une phrase → c'est mal défini

### Couche 2 — Méta-prompts de personnalisation

**Emplacement** : `meta-prompts/{nom}.md`

**Caractéristiques** :
- Prompts à copier-coller dans une nouvelle conversation Claude
- Interview structurée (8 questions max, une par une)
- Lecture MCP de l'outil cible (lecture seule, anonymisée)
- Synthèse + validation utilisateur
- Génération d'une skill custom au format Anthropic

**Pourquoi cette couche est notre angle unique** :
- Les skills de Couche 1 sont génériques
- Chaque entreprise a sa spécificité (HubSpot configuré différemment, voix différente, conventions internes)
- Les méta-prompts permettent à chaque entreprise de **générer SA skill custom** sans nous payer pour la consulter
- C'est le pattern "donner la canne à pêche au lieu du poisson", appliqué au niveau méta

**Quand créer un nouveau méta-prompt** :
- Un outil B2B est intégré via MCP et a une configuration variable d'une entreprise à l'autre (HubSpot, Lemlist, ClickUp, Pipedrive...)
- L'expertise pour configurer cet outil mériterait une skill, mais doit être calibrée par entreprise

### Couche 3 — Playbooks d'orchestration

**Emplacement** : `playbooks/{nom}.md`

**Caractéristiques** :
- Séquences de prompts numérotés, à copier-coller dans Claude.ai
- Cibles non-techniciens (commerciaux, marketeurs PME)
- Structure en phases (Phase 0 / Phase 1 / etc.) consommables individuellement
- Validation humaine systématique à chaque étape
- Mention claire de "Si tu n'as que 30 min, fais X et Y"

**Différence avec une skill** :
- Une **skill** est invoquée par Claude automatiquement
- Un **playbook** est suivi manuellement par l'utilisateur, étape par étape

**Quand créer un playbook** :
- Une procédure orchestre plusieurs skills dans un ordre précis
- Le contexte est trop variable pour automatiser (set-up CRM, audit complet)
- L'utilisateur cible n'a pas les compétences techniques pour piloter Claude librement

### Couche 4 — Références méthodologiques

**Emplacement** : `references/{sujet}.md`

**Caractéristiques** :
- Documents longs, structurés, denses
- Pas de format d'invocation Claude — c'est de la doc de référence
- Utilisable comme contexte permanent dans un Project Claude
- Source par défaut quand une skill renvoie vers un cadre théorique

**Exemples possibles** :
- `reference-jtbd-strategyn-methodology.md` (la méthode Ulwick complète)
- `reference-rgpd-b2b-france.md` (le détail réglementaire RGPD français)
- `reference-buying-committee-b2b-industrial.md` (les rôles types de buying committee industriel)
- `reference-cold-email-benchmarks-2026.md` (les benchmarks à jour)

**Quand créer une référence** :
- L'information est dense, structurée, durable (pas une opinion qui change)
- Plusieurs skills s'appuient sur cette même base
- Inutile de la dupliquer dans chaque skill

---

## Le pattern Coquille/Hub appliqué

### Origine du pattern

Ce pattern vient des architectures Kōcori SaaS de Kōeki Agency. L'idée : dans un système modulaire, séparer ce qui est **transversal et stable** (la Coquille) de ce qui est **spécifique et changeant** (les Hubs).

### Application dans ce repo

**La Coquille (`shell/SHELL.md`)** définit :
- La voix éditoriale (tutoiement, phrases courtes, vocabulaire)
- Les anti-patterns universels (mots bannis, formulations interdites)
- Les garde-fous RGPD universels
- Le format de sortie standardisé
- La frontière éthique cognitive (biais éthiques vs manipulation)
- Le pattern du mode socratique

**Les Hubs (les skills, méta-prompts, playbooks)** ne contiennent QUE ce qui leur est spécifique. Ils référencent la Coquille pour le reste.

### Concrètement, comment Claude utilise ça

Quand Claude active une skill, il lit :
1. La Coquille (héritage permanent, dans le contexte global du Project ou via instruction)
2. Le SKILL.md de la skill activée

La skill ne re-décrit pas la voix. Elle dit juste "applique la voix Kōeki définie dans shell/SHELL.md".

Si la Coquille évolue, toutes les skills héritent automatiquement. Pas de modification en cascade.

### Comment l'écrire dans une nouvelle skill

Dans le corps du SKILL.md, tu peux mentionner :
> Cette skill hérite des règles de voix et anti-patterns universels définis dans shell/SHELL.md. Elle ajoute les spécificités suivantes : [...]

Et tu ne re-décris PAS la voix générale. Juste ce qui est spécifique à cette skill.

---

## L'évaluation continue (le pattern eval)

**Emplacement** : `evals/{skill-name}/evals.json` + `evals/EVAL_FRAMEWORK.md`

Chaque skill du repo est livrée avec ses tests. C'est ce qui sépare une bibliothèque sérieuse d'une collection de prompts.

### Pourquoi c'est important

- En 2026, la communauté Claude Skills commence à peine à évaluer ses productions
- Une skill "marche" subjectivement, mais quand on lance 10 prompts variés, on découvre qu'elle ne se déclenche que sur 3
- Sans evals, tu raffines à l'aveugle : tu changes la description, tu ne sais pas si tu améliores ou tu dégrades

### Notre méthode

Pour chaque skill, on définit :
1. **3 à 5 prompts de test représentatifs** (formulations variées que de vrais utilisateurs pourraient écrire)
2. **Pour chaque prompt** : la sortie attendue (qualitative) + 2-3 assertions vérifiables programmatiquement
3. **Run AVEC et SANS la skill** (baseline) pour mesurer l'apport réel
4. **Score qualitatif + quantitatif** consigné dans le repo

Pattern inspiré du `skill-creator` officiel d'Anthropic, adapté à notre contexte.

Détail complet dans `evals/EVAL_FRAMEWORK.md`.

---

## La maïeutique comme principe transversal

Ce n'est pas une couche, c'est un **principe** qui irrigue les 4 couches.

### Dans la Couche 1 (skills)
Chaque skill a un mode socratique activable. Au lieu de produire l'output, Claude pose les questions structurées qui font émerger la connaissance chez l'utilisateur.

### Dans la Couche 2 (méta-prompts)
Les méta-prompts SONT par nature socratiques : ils posent une interview avant de produire. Modèle de référence pour le mode socratique des skills.

### Dans la Couche 3 (playbooks)
Les playbooks insèrent à chaque phase une question de réflexion pour l'utilisateur, pas juste un prompt à exécuter aveuglément.

### Dans la Couche 4 (références)
Les références incluent des questions de réflexion à la fin de chaque section. Pas que de la doc passive.

### Le rôle de l'agent dédié

`agents/agent-socratic-mode.md` est l'agent qui peut être invoqué par toute autre tâche pour vérifier qu'un contenu produit respecte le principe maïeutique.

---

## Comment évolue cette architecture

L'architecture est censée durer. Toute évolution structurelle (ajouter une couche, modifier la Coquille fondamentalement, changer le pattern d'évaluation) demande :
1. Une RFC (Request for Comments) en branche dédiée
2. Une discussion explicite avec Sébastien
3. Une mise à jour cohérente de ce fichier ARCHITECTURE.md ET de CLAUDE.md

Les évolutions de contenu (nouvelles skills, nouveaux méta-prompts, nouvelles références) suivent simplement le workflow normal de contribution.

---

## Liens utiles

- [README.md](./README.md) — vitrine SEO et présentation
- [CLAUDE.md](./CLAUDE.md) — brief permanent Claude Code
- [CONTENT_MANIFEST.md](./CONTENT_MANIFEST.md) — roadmap de contenu
- [shell/SHELL.md](./shell/SHELL.md) — la Coquille
- [evals/EVAL_FRAMEWORK.md](./evals/EVAL_FRAMEWORK.md) — méthode d'évaluation
