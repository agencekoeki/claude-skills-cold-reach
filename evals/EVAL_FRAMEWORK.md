
# EVAL_FRAMEWORK.md — Méthode d'évaluation des skills

> Comment on évalue les skills de Kōeki Claude Skills B2B FR. Pas de skill publiée sans evals. C'est ce qui sépare une bibliothèque sérieuse d'une collection de prompts.

---

## Pourquoi évaluer

En 2026, l'écosystème Claude Skills commence à peine à mesurer ses productions. La plupart des bibliothèques (Tom Granot, Lemlist, OpenClaudia) publient des skills sans evals. Résultat :
- Personne ne sait si une skill "marche" objectivement
- Quand on modifie une description, on ne sait pas si on améliore ou on dégrade
- Les contributeurs ne peuvent pas vérifier que leurs modifications ne cassent rien

L'évaluation continue est ce qui rend ce repo **professionnel** plutôt qu'artisanal.

Pattern inspiré du `skill-creator` officiel d'Anthropic (`eval-viewer/generate_review.py`), adapté à notre contexte francophone B2B.

---

## Les 3 questions auxquelles l'évaluation répond

1. **La skill se déclenche-t-elle quand elle devrait ?** (sensibilité)
2. **La skill ne se déclenche-t-elle PAS quand elle ne devrait pas ?** (spécificité)
3. **Quand elle se déclenche, sa sortie est-elle vraiment meilleure que sans elle ?** (apport réel)

Les trois questions sont distinctes. Une skill peut bien se déclencher mais produire un output médiocre. Ou bien produire un excellent output mais ne jamais se déclencher.

---

## Structure d'un fichier evals.json

Chaque skill a son fichier `evals/{skill-name}/evals.json` :

```json
{
  "skill_name": "b2b-cold-copy-psy",
  "version": "1.0",
  "last_run": "2026-05-11T18:30:00Z",
  "description": "Tests d'évaluation pour la skill cold-copy-psy",
  "tests": [
    {
      "id": "test-easy-001",
      "category": "facile",
      "rationale": "Formulation directe avec les mots-clés exacts de la description",
      "prompt": "Réécris-moi ce cold email pour qu'il soit plus persuasif",
      "context": "L'utilisateur partage un email à un acheteur Safran",
      "expected_behavior": "Skill déclenchée + production de 2 variantes A/B + voix Kōeki + anti-patterns évités",
      "assertions": [
        {
          "type": "structure",
          "check": "contains_section",
          "value": "Anti-patterns évités",
          "required": true
        },
        {
          "type": "structure",
          "check": "contains_n_variants",
          "value": 2,
          "required": true
        },
        {
          "type": "behavior",
          "check": "llm_judge",
          "criterion": "Respecte la voix Kōeki (tutoiement, phrases courtes, pas d'anglicismes)",
          "required": true
        },
        {
          "type": "behavior",
          "check": "llm_judge",
          "criterion": "Aucune manipulation cognitive interdite (fausse urgence, fausse rareté)",
          "required": true
        }
      ]
    },
    {
      "id": "test-neutral-001",
      "category": "neutre",
      "rationale": "Formulation utilisateur sans mots-clés directs",
      "prompt": "Mon email à ce prospect aéronautique sonne plat, j'ai besoin d'aide",
      "expected_behavior": "Skill déclenchée malgré l'absence des mots-clés directs",
      "assertions": [...]
    },
    {
      "id": "test-edge-001",
      "category": "limite",
      "rationale": "Cas proche du périmètre mais qui ne devrait PAS trigger",
      "prompt": "Comment je gère ma boîte mail efficacement ?",
      "expected_behavior": "Skill NON déclenchée (faux positif à éviter)",
      "assertions": [
        {
          "type": "behavior",
          "check": "skill_not_triggered",
          "required": true
        }
      ]
    },
    {
      "id": "test-socratic-001",
      "category": "socratique",
      "rationale": "Test du mode socratique",
      "prompt": "Mode socratique sur mon cold email à ce prospect Safran",
      "expected_behavior": "Skill déclenchée en mode socratique : pose 5-7 questions, ne produit pas l'email",
      "assertions": [
        {
          "type": "behavior",
          "check": "llm_judge",
          "criterion": "Claude pose des questions au lieu de produire l'email",
          "required": true
        },
        {
          "type": "behavior",
          "check": "llm_judge",
          "criterion": "Les questions suivent la structure des 4 niveaux (diagnostic, hypothèse, décision, méta)",
          "required": true
        }
      ]
    }
  ],
  "last_result": {
    "with_skill": {
      "pass_rate": 0.92,
      "details": "11/12 assertions passées sur 5 tests"
    },
    "without_skill": {
      "pass_rate": 0.25,
      "details": "3/12 assertions passées sur 5 tests"
    },
    "delta": "+0.67",
    "recommendation": "PASS — skill apporte un gain significatif vs baseline"
  }
}
```

---

## Les 5 catégories de tests

Pour chaque skill, on définit 5 tests minimum, un par catégorie :

### Catégorie 1 — "Facile"
Formulation directe avec les mots-clés exacts de la description YAML. Vérifie que la skill se déclenche dans le cas évident.

### Catégorie 2 — "Neutre"
Formulation utilisateur quotidienne, sans mots-clés directs. Vérifie que la skill se déclenche aussi sur du vocabulaire naturel.

### Catégorie 3 — "Difficile"
Formulation indirecte qui devrait quand même trigger. Vérifie la robustesse de la description aux formulations imprévues.

### Catégorie 4 — "Limite" (test du faux positif)
Cas proche du périmètre mais qui ne devrait PAS trigger. Vérifie qu'on n'a pas sur-trigger.

### Catégorie 5 — "Socratique"
Cas explicite d'activation du mode socratique. Vérifie que le mode fonctionne et qu'il pose les bonnes questions.

Tests additionnels possibles selon la skill :
- **Cas concret** : situation réelle avec contexte fourni
- **Cas multilingue** : utilisateur écrit moitié français moitié anglais
- **Cas sensible** : sujet à fort enjeu RGPD ou éthique
- **Cas de surcharge** : utilisateur donne beaucoup de contexte, on vérifie qu'on ne dérive pas

---

## Les 2 types d'assertions

### Type 1 — Assertions de structure (vérifiables programmatiquement)

Vérifient des propriétés objectives de la sortie :
- `contains_section` : la sortie contient une section nommée
- `contains_n_variants` : la sortie propose N variantes
- `contains_keywords` : la sortie contient certains mots-clés
- `excludes_keywords` : la sortie n'utilise PAS certains mots interdits (anglicismes, manipulation)
- `length_max` / `length_min` : la sortie respecte une longueur cible
- `skill_triggered` / `skill_not_triggered` : la skill s'est déclenchée ou pas

Avantage : reproductibles, automatisables. Désavantage : ne capturent pas la nuance qualitative.

### Type 2 — Assertions de comportement (LLM judge)

Vérifient des propriétés qualitatives, jugées par un autre Claude utilisé comme "juge" :
- `respecte la voix Kōeki`
- `n'utilise aucune manipulation cognitive interdite`
- `aborde le bon angle métier pour le secteur mentionné`
- `propose des variantes vraiment distinctes (pas du paraphrase)`
- `est calibré sur le niveau hiérarchique du destinataire`

Avantage : capturent la qualité réelle. Désavantage : un peu coûteuses en tokens, et peuvent être instables d'une run à l'autre.

---

## Process d'évaluation (workflow)

### Run baseline (sans la skill)

1. Charger Claude sans aucune skill
2. Coller le prompt de test
3. Capturer la sortie
4. Évaluer chaque assertion sur cette sortie
5. Calculer le pass rate baseline

### Run avec la skill

1. Charger Claude avec la skill activée
2. Coller le même prompt de test
3. Capturer la sortie
4. Évaluer chaque assertion sur cette sortie
5. Calculer le pass rate avec skill

### Comparaison

- **Delta positif** : la skill apporte de la valeur quantifiable
- **Delta nul ou négatif** : la skill ne sert à rien ou nuit — à reprendre
- **Pass rate avec skill < 80%** : la skill marche mais a des défauts — à raffiner
- **Pass rate avec skill > 90%** : skill mature, prête pour merge

---

## Critères de PASS / FAIL d'une skill

### PASS (skill prête pour merge)

Tous les critères doivent être remplis :
- Au moins **4 prompts sur 5** ont leurs assertions passées avec la skill
- Le prompt "limite" ne déclenche **PAS** la skill (pas de faux positif)
- Le mode socratique a été testé et **fonctionne**
- Le delta vs baseline est positif et significatif (>20 points)
- Aucune assertion "required: true" ne fail

### À RAFFINER (skill à itérer)

Un de ces critères :
- 3 prompts sur 5 passent (au lieu de 4)
- Le mode socratique fonctionne mais pose des questions génériques (pas SPÉCIFIQUES à la skill)
- Quelques assertions "required: false" failent
- Delta vs baseline positif mais marginal

→ Retour à `agent-skill-reviewer.md` pour corrections priorité 1

### FAIL (skill à refondre)

Un de ces critères :
- Moins de 3 prompts passent
- Le prompt "limite" déclenche faussement la skill (sur-trigger)
- Le mode socratique ne fonctionne pas ou ne se déclenche pas
- Delta vs baseline nul ou négatif
- Une assertion "required: true" critique fail

→ Retour à `agent-skill-writer.md` pour refonte

---

## Maintenance dans le temps

### Re-run périodique

Tous les 3 mois, on relance les evals de TOUTES les skills :
- Pour détecter une régression (la skill ne marche plus aussi bien sur un nouveau Claude)
- Pour identifier des évolutions de comportement
- Pour rafraîchir le `last_run` et `last_result` dans les evals.json

### Re-run après modification

Toute modification d'une skill déclenche automatiquement (en CI ou manuellement) une re-run de ses evals. Si une assertion casse, la PR est bloquée.

### Évolution des evals eux-mêmes

Les evals peuvent évoluer. Si un nouveau cas d'usage apparaît sur le terrain, on l'ajoute aux evals (avec un nouveau test id). Si une assertion devient obsolète, on la marque `deprecated: true` plutôt que la supprimer (pour garder l'historique).

---

## Outillage

### Pour l'instant (v1.0)

Tests manuels : on lance les prompts dans deux conversations Claude, on capture les sorties, on évalue les assertions à la main, on consigne dans evals.json.

### Plus tard (v2.0)

Script Python (à développer) :
- Lit `evals/{skill-name}/evals.json`
- Lance chaque prompt en API Claude avec et sans la skill chargée
- Évalue les assertions (programmatiques en local + LLM judge via API)
- Met à jour le `last_run` et `last_result` dans evals.json
- Génère un rapport HTML ou Markdown

Le pattern est inspiré de `eval-viewer/generate_review.py` du skill-creator officiel d'Anthropic.

---

## Quand un contributeur soumet une PR

La PR doit contenir :
1. La modification de la skill
2. La mise à jour des evals si nécessaire (nouveaux cas, nouvelles assertions)
3. Les résultats du dernier run dans le PR_BODY

Sans evals à jour, la PR n'est pas reviewable. C'est non-négociable.

---

## Notes méthodologiques

### Sur les LLM judges

L'usage d'un LLM comme juge est fiable mais instable. Pour éviter les variations :
- Toujours utiliser le même modèle pour les jugements (Claude Sonnet 4.5 par défaut)
- Toujours utiliser le même prompt de jugement
- Faire 3 runs et prendre le résultat médian si l'assertion est critique

### Sur le coût

Evaluer une skill avec 5 tests × 2 conditions (avec/sans) × quelques assertions LLM judge = ~10-15 appels API. Coût marginal mais non nul. Pour des skills critiques, ça vaut le coup. Pour des skills marginales, on peut se limiter à 3 tests.

### Sur la subjectivité

Toute évaluation a une part de subjectivité. On l'accepte. Le but n'est pas la mesure objective absolue mais une mesure relative reproductible qui détecte les régressions.

---

**Made with 📊 — La méthode d'éval Kōeki v1.0**
