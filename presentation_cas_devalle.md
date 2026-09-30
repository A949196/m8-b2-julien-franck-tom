---
marp: true
theme: gaia
_class: lead
paginate: true
backgroundColor: #f8fafc
color: #0f172a
header: "Cabinet Maître Devalle — Architecture Cible & Décision IA"
footer: "M8-B2 — Franck, Julien, Tom · Octobre 2026"
style: |
  section {
    font-size: 24px;
    padding: 35px 50px 30px 50px;
  }
  h1 {
    font-size: 42px;
  }
  h2 {
    font-size: 32px;
    margin-bottom: 15px;
    color: #1e3a8a;
  }
  h3 {
    font-size: 24px;
    margin-bottom: 12px;
  }
  ul, ol {
    margin-top: 5px;
    margin-bottom: 10px;
  }
  li {
    margin-bottom: 8px;
    line-height: 1.35;
  }
  table {
    font-size: 19px;
  }
  th, td {
    padding: 8px 12px;
  }
  pre {
    font-size: 17px;
    margin-top: 5px;
    margin-bottom: 10px;
    padding: 10px;
  }
---

# Cabinet Maître Devalle
### Modernisation de la recherche juridique interne (Cas A)

**Dossier d'arbitrage et d'architecture cible**  
*Franck, Julien, Tom*

---

## 1. Présentation du Cas : Contexte

**Cabinet d'affaires & contentieux (TJ Bordeaux)**

- **Équipe :** 12 avocats associés et collaborateurs.
- **Fonds documentaire :**
  - **2 000 décisions** internes archivées depuis 2010.
  - Documents scannés ou hétérogènes, mal nommés (`decision_1004.pdf`).
  - Base de courriers types datant de 2019 (obsolète, non maintenue).
- **Usage réel :** ~10 recherches/jour ouvré (~220 requêtes/mois).
- **Urgence critique au 31/12 :** Fin du contrat du prestataire IT sans administrateur système interne.

---

## 2. La Problématique Métier

**Goulot d'étranglement documentaire et perte d'heures facturables**

- **Temps perdu :** ~30 min par recherche avec les outils actuels (Windows/grep).
- **Surcharge cognitive :** Obligation d'ouvrir et lire 3 à 4 décisions de 15 pages pour vérifier leur pertinence.
- **Dette documentaire :** Modèles 2019 obsolètes $\rightarrow$ risque direct d'erreur de droit en cas de réutilisation non vérifiée.
- **Objectif d'usage :** Retrouver la bonne jurisprudence interne en moins de 2 minutes avec justification factuelle.

---

## 3. Les Risques & Contraintes Clés

**Un cadre réglementaire et d'exploitation sans compromis**

- **Secret professionnel (art. 66-5) :** Exclusion des LLM publics US (CLOUD Act) $\rightarrow$ Cloud souverain SecNumCloud FR.
- **Données sensibles (RGPD art. 9) :** Neutralisation du risque par exclusion formelle du droit de la famille en V1.
- **Péril d'exploitation au 31/12 :** Serveur local vieillissant sans sauvegarde testée $\rightarrow$ impératif de migration managée.
- **Cadre budgétaire :** Plafond strict de **15 000 € Build** et < **200 €/mois Run**.

---

## 4. Synthèse des 5 Arbitrages Clés (Sobriété)

| Arbitrage | Choix Retenu | Justification Principale |
|---|---|---|
| **ML vs Deep Learning** | **Deep Learning (Embeddings)** | Synonymie juridique sur 2 000 décisions non labellisées. |
| **SLM vs LLM** | **SLM souverain (7B-8B)** | Synthèse factuelle en 3 phrases, coût < 20 €/mois. |
| **RAG oui / non** | **OUI (Ancrage strict)** | Corpus interne fermé, zéro tolérance d'hallucination. |
| **Agents oui / non** | **NON (Pipeline linéaire)** | Tâche déterministe, latence < 2 s, 0 risque d'effet de bord. |
| **Zero-shot suffit ?** | **OUI (Prompt strict)** | Économie de 15 j·h d'annotation (10 000 € préservés). |

---

## 5. Arbitrages de Périmètre (Divergences Tranchées)

**Priorisation par la valeur et la maîtrise du risque**

1. **Recherche seule en V1 (Exclusion des courriers types) :**
   - Évite d'automatiser sur des gabarits 2019 obsolètes.
   - Répond à la priorité n°1 exprimée par le client.
2. **Exclusion formelle du droit de la famille :**
   - Élimine le risque art. 9 RGPD (santé, mineurs) sans bloquer le lancement.
   - Baux commerciaux et recouvrement = > 50 % du volume utile.
3. **Gain de temps réaliste :**
   - Cible V1 fixée à **20-25 min / jour / avocat** (calcul mathématique réel).

---

## 6. Architecture Cible V1

```
[Avocat / Auth MFA] ---> [Frontend Web & API FastAPI (SecNumCloud FR)]
                               |                     |
             +-----------------+                     +-----------------+
             v                                                         v
   [Base pgvector FR]                                         [SLM Managé FR]
(Embeddings bge-m3 sur S3 FR)                              (Synthèse en 3 phrases)
```

- **Ingestion batch sécurisée :** OCR + filtre RGPD + désensibilisation PII.
- **Moteur en ligne :** Retrieval vectoriel bge-m3 + seuil de confiance (0.72).
- **Garde-fous :** Fallback nominal automatique si pertinence insuffisante.
- **Déontologie :** Lien direct PDF et **validation humaine obligatoire**.

---

## 7. Le Jalon 0 : Réponse à l'Imprévu du 31/12

**Sécuriser le socle technique avant toute phase de Build applicatif**

- **Avant le 31 décembre :**
  1. Test réel d'une restauration complète des sauvegardes locales.
  2. Récupération documentée des accès racine et certificats.
  3. Révocation formelle des accès du prestataire sortant.
  4. Copie chiffrée des 2 000 décisions vers un bucket S3 souverain.
- **Résultat :** Dès le départ du prestataire, le cabinet fonctionne sur un PaaS managé ne nécessitant aucun administrateur interne.

---

## 8. Chiffrage & Maîtrise des Coûts

**Respect strict des plafonds budgétaires du cabinet**

- **Coûts de Build : 14 400 € TTC** (Budget max : 15 000 €)
  - Jalon 0 & Migration IT : 1 800 € (2 j·h)
  - Ingestion, OCR & Pipeline RAG : 9 000 € (10 j·h)
  - Interface Web, Sécurité & Recette : 3 600 € (4 j·h)
- **Coûts de Run récurrent : ~80 € / mois**
  - Stockage S3 & base pgvector : ~25 € / mois
  - Inférence SLM managé souverain : ~15 € / mois
  - Hébergement conteneur & sauvegardes : ~40 € / mois

---

## 9. Évaluation & Qualification Réglementaire

**Garanties techniques et juridiques pour le cabinet**

- **Protocole d'évaluation :**
  - Banc de test « Golden Dataset » : **50 requêtes réelles** d'avocats associés.
  - Objectifs : *Recall@3* $\ge 90\,\%$, *Fidélité au contexte (Groundedness)* = $100\,\%$.
- **Cadre AI Act :**
  - Système interne d'aide à la recherche : **hors Annexe III**.
  - Respect de l'obligation de transparence (art. 50) et formation (art. 4).
- **Responsabilité civile :** L'avocat reste le seul décisionnaire et signataire.

---

## 10. Conclusion : Les 4 Forces de la Solution

- **1. Opérationnelle immédiatement :** Neutralise la bombe à retardement du 31 décembre grâce au Jalon 0.
- **2. Souveraine et déontologique :** Données hébergées en France, secret professionnel sanctuarisé.
- **3. Frugale et sobre :** Zéro agent inutile, pas de fine-tuning coûteux, 80 €/mois de Run.
- **4. Honnête et mesurable :** 20-25 min gagnées/jour/avocat sans fausse promesse sur les courriers types.

**Prochaine étape :** Recette sur le Golden Dataset de 50 requêtes et validation du Jalon 0.
