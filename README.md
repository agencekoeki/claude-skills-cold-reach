# claude-skills-cold-reach
> 🇫🇷 La première bibliothèque francophone de Claude Skills pour la prospection B2B éthique. Architecture en couches, mode socratique intégré, voix industrielle française.

[![Skills count](https://img.shields.io/badge/skills-7+-blue)](./skills)
[![Meta-prompts](https://img.shields.io/badge/meta--prompts-2-purple)](./meta-prompts)
[![Playbooks](https://img.shields.io/badge/playbooks-2+-green)](./playbooks)
[![License](https://img.shields.io/badge/license-MIT-green)](./LICENSE)
[![Claude.ai compatible](https://img.shields.io/badge/Claude.ai-compatible-blueviolet)](https://claude.ai)
[![Claude Code compatible](https://img.shields.io/badge/Claude%20Code-compatible-blueviolet)](https://claude.com/claude-code)
[![Made in France](https://img.shields.io/badge/Made%20in-France%20🇫🇷-blue)](https://koeki.fr)

Construit par **[Sébastien Grillot](https://www.linkedin.com/in/sebastiengrillot)** — co-fondateur de **[Kōeki Agency](https://koeki.fr)** — agence SEO & IA basée à Tarascon (PACA, France). Certifications Qualiopi, CII, France Num.

---

## Pourquoi ce repo existe

L'écosystème Claude Skills est aujourd'hui dominé par des bibliothèques anglophones, calibrées pour le SaaS américain, conçues pour Claude Code, et orientées "production déléguée" : tu demandes, Claude produit, tu valides.

Cette approche pose trois problèmes pour les équipes B2B francophones :

1. **Décalage culturel et linguistique.** Les skills anglophones imposent un ton corporate américain qui ne fonctionne pas en B2B industriel français (cycles longs, multi-décideurs, vouvoiement, certifications normatives, RGPD).

2. **Décalage technique.** Les skills SaaS ne couvrent pas les contextes industriels : NDA, marchés publics, ITAR/EAR, certifications EN 9100, contraintes export.

3. **Décalage philosophique.** La "production déléguée" peut accélérer une équipe expérimentée, mais déresponsabilise une équipe en apprentissage. Personne ne forme l'humain pendant ce temps.

Cette bibliothèque répond aux trois.

## Ce qui rend ces skills différentes

### 🇫🇷 Voix industrielle B2B française

Vouvoiement, vocabulaire technique précis, preuves avant promesses, registre adapté aux cycles longs et aux buying committees multi-décideurs. Pas de "leverage", "synergies" ou autres anglicismes commerciaux. Pas de superlatifs creux.

### 🏗️ Architecture en couches explicite

Les skills s'empilent en 4 couches qui s'enrichissent l'une l'autre :

```
Couche 1 — Expertise générique           (skills B2B universelles)
   ↓
Couche 2 — Méta-prompts                  (génèrent des skills custom par entreprise)
   ↓
Couche 3 — Playbooks d'orchestration     (séquencent l'usage des skills)
   ↓
Couche 4 — Références méthodologiques    (matière brute, méthodes validées)
```

Voir [ARCHITECTURE.md](./ARCHITECTURE.md) pour le détail.

### 🐚 Pattern Coquille/Hub (zéro redondance)

Chaque skill hérite d'une **Coquille** commune (voix, anti-patterns universels, garde-fous RGPD, format de sortie). La skill elle-même ne contient que ce qui lui est spécifique. Résultat : mise à jour de la voix sans toucher 30 fichiers, cohérence garantie entre toutes les skills.

Pattern hérité des architectures Kōcori SaaS de Kōeki Agency.

### 🧠 Mode socratique intégré (différenciation principale)

C'est notre vraie singularité. Sur chaque skill, un mode **socratique** est activable. Au lieu de produire l'output, Claude pose les questions structurées qui font émerger la connaissance chez l'utilisateur. Tu sors de la session avec une compétence renforcée, pas seulement avec un livrable.

> Ces skills sont conçues pour devenir un peu moins utiles à chaque fois que tu les utilises.

Voir [agents/agent-socratic-mode.md](./agents/agent-socratic-mode.md) pour le détail.

### 📊 Évaluation continue intégrée

Chaque skill est livrée avec ses test cases (`evals/skill-name.json`) et un benchmark before/after. Pas de skill publiée sans validation quantitative. Pattern inspiré du `skill-creator` officiel d'Anthropic.

Voir [evals/EVAL_FRAMEWORK.md](./evals/EVAL_FRAMEWORK.md) pour le détail.

### ⚖️ Éthique cognitive explicite

Frontière claire entre biais cognitifs éthiques (réciprocité, preuve sociale, autorité) et manipulation (fausse rareté, fausse urgence, manipulation émotionnelle). Documentée par skill, vérifiable par utilisateur.

### 🇪🇺 RGPD français/européen explicite

Couverture complète : base légale d'intérêt légitime B2B, mentions obligatoires, droits du destinataire (accès, opposition, effacement), cas particuliers industriels (NDA, marchés publics, défense, ITAR/EAR).

---

## Installation

### Sur Claude.ai (recommandé pour équipes non-techniques)

1. Va sur claude.ai → Settings → Capabilities → active "Code execution"
2. Settings → Features → Skills
3. Pour chaque skill que tu veux utiliser :
   - Télécharge le dossier de la skill depuis ce repo
   - Zippe le dossier (avec son SKILL.md à la racine du zip)
   - Upload via "Upload Skill"

Ou utilise les skills comme **knowledge base** dans un Claude Project (méthode plus simple, voir [playbooks/setup-claude-project.md](./playbooks/setup-claude-project.md)).

### Sur Claude Code (recommandé pour développeurs)

```bash
git clone https://github.com/sebastiengrillot/koeki-claude-skills-b2b-fr.git
cp -r koeki-claude-skills-b2b-fr/skills/* ~/.claude/skills/
```

Ou comme plugin :
```bash
/plugin marketplace add sebastiengrillot/koeki-claude-skills-b2b-fr
/plugin install koeki-b2b@koeki-claude-skills-b2b-fr
```

---

## Skills disponibles

### Couche 1 — Skills d'expertise B2B générique

| Skill | Description | Mode socratique |
|---|---|---|
| `b2b-personas-jtbd` | Construit des personas B2B avec Jobs-To-Be-Done + buying committee | ✅ |
| `b2b-lead-magnet` | Conçoit des lead magnets B2B qui convertissent en 2026 | ✅ |
| `b2b-cold-copy-psy` | Cold copy persuasif éthique (frameworks + biais cognitifs) | ✅ |
| `b2b-reply-handling` | Gestion des replies prospects (11 types d'objections) | ✅ |
| `hubspot-pipeline-b2b` | Pipelines HubSpot B2B propres + hygiène + forecast | ✅ |
| `lemlist-deliverability` | Délivrabilité Lemlist (Lemwarm, SPF/DKIM/DMARC, domaines) | ✅ |
| `lemlist-audit-campagne` | Audit de campagne Lemlist en 5 dimensions | ✅ |

### Couche 2 — Méta-prompts (génèrent des skills custom)

| Méta-prompt | Description |
|---|---|
| `meta-prompt-hubspot-skill-creator` | Interview + lecture HubSpot via MCP → skill HubSpot personnalisée par entreprise |
| `meta-prompt-lemlist-skill-creator` | Interview + lecture Lemlist via MCP → skill Lemlist personnalisée par entreprise |

### Couche 3 — Playbooks d'orchestration

| Playbook | Description |
|---|---|
| `playbook-setup-crm-outreach` | Mettre HubSpot au carré en 6 phases avant lancement d'une campagne outbound |
| `playbook-setup-lemlist-outreach` | Mettre Lemlist au carré avant lancement d'une campagne outbound |

### Couche 4 — Références méthodologiques

Documents de référence (frameworks, benchmarks, méthodes validées) utilisables comme contexte permanent dans les Projects Claude. Voir [references/](./references).

---

## Pour qui c'est conçu

### Cibles principales
- **Agences SEO / IA** francophones qui accompagnent des clients B2B
- **Organismes de formation Qualiopi** qui forment à l'IA appliquée au marketing
- **Équipes commerciales et marketing en PME industrielle B2B** française
- **Consultants indépendants** en growth, RevOps, marketing B2B
- **Formateurs IA** qui cherchent du matériel pédagogique calibré pour leurs stagiaires

### À éviter si
- Tu veux générer du B2C, e-commerce généraliste, ou crypto
- Ton marché cible est anglophone à 100%
- Tu cherches une bibliothèque exhaustive multi-domaines (préfère [Composio awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) ou [Anthropic skills](https://github.com/anthropics/skills))

---

## L'auteur

**Sébastien Grillot** est co-fondateur de [**Kōeki Agency**](https://koeki.fr), agence SEO et IA basée à Tarascon (Provence-Alpes-Côte d'Azur, France).

Plus de 16 ans d'expérience en SEO depuis 2008/2009. Master 2 en Management de Projets Informatiques (ESIEE-IT). Linux depuis 2000. Intervenant WordCamp Paris 2015 et WordCamp Bordeaux 2017.

Kōeki Agency est certifiée **Qualiopi** (formation professionnelle), **CII** (Crédit d'Impôt Innovation), **France Num**, et accompagne les organismes de formation, CCI, PME industrielles et plateformes médicales sur leurs stratégies SEO et IA.

**Spécialités** : formation IA appliquée (CCI, BIP Info Pro, Structa Industries, Alfie Formation), SEO YMYL/E-E-A-T pour plateformes santé (Hospitalidée), prospection outbound B2B éthique, automatisation marketing via Claude Code et MCP.

**Projets en cours** :
- **Kōcori SaaS** — dashboard SEO français avec orchestration HubSpot + ClickUp + Gmail via MCP
- **Archipel SSG** — générateur de sites statiques PHP pour PBN/money sites
- **Boostacademy.ai** — catalogue de formations IA (build in public)
- **Livre "0 Clic"** — l'impact de l'IA sur le trafic web et le zero-click search

**Contact** : [sebastien@koeki.fr](mailto:sebastien@koeki.fr) | [LinkedIn](https://www.linkedin.com/in/sebastiengrillot) | [Twitter/X](https://twitter.com/sebastiengrillot) | [koeki.fr](https://koeki.fr)

---

## Roadmap

- **v1.0** (en cours) — 7 skills d'expertise B2B + 2 méta-prompts + 2 playbooks + framework d'évaluation
- **v1.1** (Q3 2026) — Skills socratiques pures (positionnement, stratégie Q, debrief formation)
- **v1.2** (Q4 2026) — Intégration MCP avancée (LinkedIn, Calendar, Drive)
- **v2.0** (2027) — Extension vers le SEO francophone (skills E-E-A-T, YMYL, content GEO)

---

## Contribuer

Voir [CONTRIBUTING.md](./CONTRIBUTING.md). Toutes les contributions sont bienvenues, en particulier :
- Nouvelles skills pour secteurs B2B francophones spécifiques (santé, défense, aéronautique, énergie)
- Skills socratiques pures (qui ne produisent pas, mais font penser)
- Tests d'évaluation supplémentaires
- Traductions vers d'autres langues francophones (Québec, Belgique, Suisse, Afrique)

---

## License

MIT — voir [LICENSE](./LICENSE).

---

## Citations et inspirations

Ce repo s'inspire (sans copier) du travail des pionniers de l'écosystème Claude Skills 2025-2026 :
- **Anthropic** — [skill-creator officiel](https://github.com/anthropics/skills) et standard Agent Skills
- **Tom Granot** — [hubspot-admin-skills](https://github.com/TomGranot/hubspot-admin-skills) pour la logique audit/plan/execute/maintain
- **Lemlist** — [lemlist.com/claude-skills](https://www.lemlist.com/claude-skills) pour les playbooks B2B éprouvés sur 244K+ campagnes
- **Joseph Deville** — [GTMClaudSkills](https://github.com/josephdeville/GTMClaudSkills) pour l'approche GTM engineering

L'angle francophone industriel B2B, le pattern Coquille/Hub, le mode socratique et l'éthique cognitive explicite sont propres à Kōeki Agency.

---

**Made with ❤️ et 🤔 in Tarascon, Provence, France.**

*Une production [Kōeki Agency](https://koeki.fr) — par [Sébastien Grillot](https://www.linkedin.com/in/sebastiengrillot).*
