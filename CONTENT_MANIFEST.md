
# CONTENT_MANIFEST.md — Roadmap de contenu du repo

> Liste structurée de tout ce qui doit aller dans le repo. Sert de feuille de route pour Claude Code à chaque session. Tu lis ce fichier au démarrage, tu vois où on en est, tu proposes par où on continue.

---

## Légende des statuts

- ✅ **DONE** : produit, testé, mergé sur `main`
- 🚧 **WIP** : production en cours dans une branche
- 📝 **TO PRODUCE** : à produire, contenu disponible (transcripts antérieurs, skills déjà rédigées hors repo)
- 💡 **TO DESIGN** : à concevoir from scratch
- ⏸️ **PAUSED** : production démarrée puis suspendue, à reprendre

---

## Couche 0 — Fondations du repo

Ces fichiers sont l'infrastructure. Ils étaient déjà produits dans la conversation d'amorce.

| Fichier | Statut | Source | Note |
|---|---|---|---|
| `README.md` | ✅ DONE | Conv. amorce | Vitrine SEO avec vectorisation Sébastien Grillot + Kōeki Agency |
| `CLAUDE.md` | ✅ DONE | Conv. amorce | Brief permanent CC |
| `ARCHITECTURE.md` | ✅ DONE | Conv. amorce | Doc 4 couches + Coquille/Hub + maïeutique |
| `shell/SHELL.md` | ✅ DONE | Conv. amorce | La Coquille v1.0 |
| `agents/agent-skill-writer.md` | ✅ DONE | Conv. amorce | Sous-agent écriture |
| `agents/agent-skill-reviewer.md` | ✅ DONE | Conv. amorce | Sous-agent revue |
| `agents/agent-skill-tester.md` | ✅ DONE | Conv. amorce | Sous-agent test/eval |
| `agents/agent-socratic-mode.md` | ✅ DONE | Conv. amorce | Sous-agent maïeutique |
| `evals/EVAL_FRAMEWORK.md` | 📝 TO PRODUCE | Conv. amorce | Méthode d'évaluation |
| `CONTRIBUTING.md` | 📝 TO PRODUCE | Conv. amorce | Guide de contribution |
| `LICENSE` | 💡 TO DESIGN | — | MIT — fichier standard à générer |
| `CHANGELOG.md` | 💡 TO DESIGN | — | À démarrer au premier merge |
| `.gitignore` | 💡 TO DESIGN | — | Standard + spécificités du repo |

---

## Couche 1 — Skills d'expertise B2B générique

Ces 7 skills ont été rédigées en amont, en dehors du repo. Elles doivent être reprises, adaptées à la Coquille (héritage), enrichies du mode socratique, et testées.

### Skills à reprendre depuis conversations antérieures

| Skill | Statut | Source du contenu | Action requise |
|---|---|---|---|
| `skills/b2b-personas-jtbd/SKILL.md` | 📝 TO PRODUCE | Skill `01_b2b-personas-jtbd.md` (sortie conv. antérieure) | Reprendre + appliquer Coquille + ajouter mode socratique + tests |
| `skills/b2b-lead-magnet/SKILL.md` | 📝 TO PRODUCE | Skill `02_lead-magnet-b2b.md` | Idem |
| `skills/b2b-cold-copy-psy/SKILL.md` | 📝 TO PRODUCE | Skill `03_cold-copy-psy.md` | Idem |
| `skills/b2b-reply-handling/SKILL.md` | 📝 TO PRODUCE | Skill `07_b2b-reply-handling.md` | Idem |
| `skills/hubspot-pipeline-b2b/SKILL.md` | 📝 TO PRODUCE | Skill `04_hubspot-pipeline-b2b.md` | Idem |
| `skills/lemlist-deliverability/SKILL.md` | 📝 TO PRODUCE | Skill `05_lemlist-deliverability.md` | Idem |
| `skills/lemlist-audit-campagne/SKILL.md` | 📝 TO PRODUCE | Skill `06_lemlist-audit-campagne.md` | Idem |

### Process pour chaque skill reprise (workflow recommandé)

Pour chaque skill, séquence type avec CC :
1. **Prompt 1** : "Lis le contenu source (que je colle), produis la v1 du SKILL.md en appliquant la Coquille et l'agent-skill-writer.md"
2. **Prompt 2** : "Lance agent-skill-tester.md sur cette v1. Génère 5 prompts de test et evals.json"
3. **Prompt 3** (si besoin) : "Lance agent-skill-reviewer.md sur cette v1, corrige les points priorité 1"
4. **Prompt 4** : "Commit dans la branche skill/{nom} et prépare la PR vers dev"

Estimation : 4 prompts par skill × 7 skills = **28 prompts** pour cette couche.

### Skills socratiques pures (v1.1 — à concevoir)

Skills qui n'ont QUE le mode socratique, pas de production. Pour la version 1.1 du repo.

| Skill | Statut | Description |
|---|---|---|
| `skills/socratic-positionnement/SKILL.md` | 💡 TO DESIGN | 10 questions pour construire SON positionnement |
| `skills/socratic-strategie-trimestre/SKILL.md` | 💡 TO DESIGN | 8 questions pour cadrer sa stratégie Q |
| `skills/socratic-debrief-formation/SKILL.md` | 💡 TO DESIGN | 6 questions pour débriefer une journée de formation |
| `skills/socratic-go-no-go/SKILL.md` | 💡 TO DESIGN | 7 questions pour prendre une décision oui/non |

Estimation : ~3 prompts par skill × 4 skills = **12 prompts** pour v1.1.

---

## Couche 2 — Méta-prompts

Prompts qui génèrent des skills custom par entreprise via interview + lecture MCP.

| Méta-prompt | Statut | Source | Action |
|---|---|---|---|
| `meta-prompts/meta-prompt-hubspot-skill-creator.md` | 📝 TO PRODUCE | Conv. antérieure | Reprendre + appliquer Coquille |
| `meta-prompts/meta-prompt-lemlist-skill-creator.md` | 📝 TO PRODUCE | Conv. antérieure | Reprendre + appliquer Coquille |

Estimation : 2 prompts par méta-prompt × 2 = **4 prompts**.

### Méta-prompts à concevoir (v1.2)

| Méta-prompt | Statut | Description |
|---|---|---|
| `meta-prompts/meta-prompt-clickup-skill-creator.md` | 💡 TO DESIGN | Pour ClickUp (gestion projet) |
| `meta-prompts/meta-prompt-pipedrive-skill-creator.md` | 💡 TO DESIGN | Pour Pipedrive (CRM alternatif) |
| `meta-prompts/meta-prompt-notion-skill-creator.md` | 💡 TO DESIGN | Pour Notion (knowledge base) |

---

## Couche 3 — Playbooks d'orchestration

Séquences de prompts numérotés pour les non-techniciens.

| Playbook | Statut | Source | Action |
|---|---|---|---|
| `playbooks/playbook-setup-crm-outreach.md` | 📝 TO PRODUCE | Conv. antérieure | Reprendre + appliquer Coquille |
| `playbooks/playbook-setup-lemlist-outreach.md` | 💡 TO DESIGN | À concevoir | Pendant Lemlist du playbook CRM |
| `playbooks/playbook-launch-first-campaign.md` | 💡 TO DESIGN | À concevoir | Lancement de la 1ère campagne après set-up |
| `playbooks/playbook-weekly-pipeline-review.md` | 💡 TO DESIGN | À concevoir | Revue hebdo de pipeline |

Estimation : 2 prompts par playbook × 4 = **8 prompts**.

---

## Couche 4 — Références méthodologiques

Documents denses, structurés, durables. Utilisables comme contexte permanent dans Projects Claude.

### Références à produire (v1.0)

| Référence | Statut | Description |
|---|---|---|
| `references/reference-jtbd-strategyn.md` | 💡 TO DESIGN | La méthode Ulwick JTBD complète |
| `references/reference-rgpd-b2b-france.md` | 💡 TO DESIGN | Détail RGPD français pour cold outbound |
| `references/reference-buying-committee-industrial.md` | 💡 TO DESIGN | Rôles types buying committee industriel B2B |
| `references/reference-cold-email-benchmarks-2026.md` | 💡 TO DESIGN | Benchmarks open/reply/bounce rates 2026 par secteur |
| `references/reference-frameworks-cold-copy.md` | 💡 TO DESIGN | PAS, BAB, AIDA, Signal-Based, Problem-Solution |

### Références externes à inclure (en `references/external/`)

Les 3 SKILL.md de Tom Granot à inclure comme docs externes pour les missions avancées :

| Document | Statut | Source |
|---|---|---|
| `references/external/tom-granot-hubspot-audit.md` | 💡 TO DESIGN | Download du SKILL.md depuis github.com/TomGranot/hubspot-admin-skills + adaptation francophone |
| `references/external/tom-granot-implementation-plan.md` | 💡 TO DESIGN | Idem |
| `references/external/tom-granot-weekly-cleanup.md` | 💡 TO DESIGN | Idem |

À inclure avec mention explicite de la source et licence d'origine. On adapte le contenu en français mais on attribue clairement.

Estimation : ~2 prompts par référence × 8 = **16 prompts**.

---

## Couche 5 — Documentation produite

Documentation transversale au repo.

| Doc | Statut | Description |
|---|---|---|
| `docs/installation-claude-ai.md` | 💡 TO DESIGN | Guide install sur Claude.ai web (avec captures) |
| `docs/installation-claude-code.md` | 💡 TO DESIGN | Guide install sur Claude Code |
| `docs/usage-by-persona.md` | 💡 TO DESIGN | Parcours d'usage par persona (formateur, commercial PME, RevOps, consultant) |
| `docs/voice-and-style-guide.md` | 💡 TO DESIGN | Guide de style éditorial pour contributeurs |

Estimation : ~2 prompts par doc × 4 = **8 prompts**.

---

## Récapitulatif effort total

| Couche | Skills/Files | Prompts CC estimés |
|---|---|---|
| Couche 0 (Fondations) | 2 fichiers restants (EVAL_FRAMEWORK + CONTRIBUTING) | déjà en cours |
| Couche 1 (Skills v1.0) | 7 skills | ~28 prompts |
| Couche 1 (Skills v1.1 socratiques) | 4 skills | ~12 prompts |
| Couche 2 (Méta-prompts v1.0) | 2 méta-prompts | ~4 prompts |
| Couche 3 (Playbooks v1.0) | 4 playbooks | ~8 prompts |
| Couche 4 (Références v1.0) | 8 références | ~16 prompts |
| Couche 5 (Doc) | 4 docs | ~8 prompts |
| **TOTAL v1.0 (sans v1.1 socratiques)** | | **~64 prompts** |
| **TOTAL v1.0 + v1.1** | | **~76 prompts** |

Cela représente environ **3 à 5 sessions de 2h** avec CC, étalées sur 2-3 semaines.

---

## Plan de release suggéré

### Phase 1 — Repo amorcé (1 session, 2h)
- Compléter Couche 0 (EVAL_FRAMEWORK + CONTRIBUTING + LICENSE + .gitignore)
- Premier commit sur `main`
- README en ligne, badges fonctionnels

### Phase 2 — v0.1 (skills phare, 2 sessions de 2h)
- Couche 1 : 3 skills phare (cold-copy-psy, hubspot-pipeline, lemlist-deliverability)
- Couche 2 : 2 méta-prompts
- 1 playbook (setup-crm-outreach)
- Tag v0.1

### Phase 3 — v1.0 (release publique, 2 sessions de 2h)
- Couche 1 : les 4 skills restantes
- Couche 3 : les 3 playbooks restants
- Couche 4 : les 5 références internes
- Couche 5 : doc installation
- Tag v1.0 → publication GitHub + post LinkedIn

### Phase 4 — v1.1 (skills socratiques, 1 session de 2h)
- Couche 1 : les 4 skills socratiques pures
- Tag v1.1

### Phase 5 — v1.2 (extension méta-prompts, 1 session de 2h)
- Couche 2 : 3 méta-prompts additionnels
- Couche 4 : références externes (Tom Granot adaptées)
- Tag v1.2

---

## Workflow de prompt-type avec CC

Pour produire une skill type "reprise depuis source antérieure", prompt-type :

```
Reprends le contenu de [skill source - je le colle ci-après ou je le pointe].

Applique les règles de production définies dans :
- agents/agent-skill-writer.md
- shell/SHELL.md (la Coquille)
- ARCHITECTURE.md (la couche concernée)

Produis :
1. Le SKILL.md dans skills/{nom-court}/SKILL.md
2. Vérifie l'héritage de la Coquille (pas de redondance)
3. Vérifie le mode socratique intégré
4. Vérifie le format de sortie standardisé

Ne commit pas tant que je n'ai pas validé.
Présente-moi ta v1 et attends mon GO.

[contenu source]
```

---

## Notes importantes

### Sur le maintien de ce manifest

Ce fichier doit être mis à jour à chaque commit majeur. Statuts à actualiser. Nouvelles idées à ajouter en bas de section. CC peut proposer des mises à jour mais la responsabilité finale est de Sébastien.

### Sur les statuts "TO DESIGN"

Ces items sont des idées validées par le manifest mais qui n'ont pas encore de plan détaillé. Avant de produire un "TO DESIGN", CC doit faire un mini-cadrage (3-5 questions à Sébastien) pour définir le périmètre exact.

### Sur la priorité

L'ordre de production recommandé suit le plan de release. Mais Sébastien peut piocher à sa main selon son énergie et son contexte. Le manifest est une carte, pas un itinéraire imposé.
