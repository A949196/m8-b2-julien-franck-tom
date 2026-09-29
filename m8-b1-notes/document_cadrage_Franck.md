# Document de cadrage — Cabinet Maître Devalle (3 pages max)

> **Projet :** Assistant interne de recherche de jurisprudence locale et d'aide à la rédaction de courriers  
> **Client :** Cabinet Maître Devalle (12 avocats, Bordeaux)  
> **Auteur :** Franck (Consultant IA FastIA) — Date : 29/09/2026

---

## 1. Synthèse exécutive (5-6 lignes — rédigée EN DERNIER)
Le Cabinet Maître Devalle fait face à une dispersion documentaire pénalisante et souhaite accélérer la recherche de ses décisions internes tout en fiabilisant ses courriers répétitifs (recouvrement et baux commerciaux). La solution retenue est un système documentaire augmenté (RAG sobre) combinant recherche sémantique sur métadonnées et génération encadrée via un modèle compact open source, sous contrôle humain obligatoire. L'ensemble est déployé sur un service souverain managé français (SecNumCloud) avec engagement contractuel DPA, garantissant le secret professionnel de l'avocat et une exploitation sans charge technique interne. Le projet vise un gain net d'une heure par jour par avocat et une recherche de décision en moins d'une minute, pour un budget de 15 000 € (Build) et moins de 300 €/mois (Run).

> **Imprévu client (14h30) — ce que ça change** : L'arrêt du contrat du prestataire informatique au 31 décembre supprime toute capacité de maintenance sur le serveur interne du cabinet. L'auto-hébergement on-premise devient intenable. L'arbitrage bascule vers une **infrastructure cloud souveraine managée (FR/UE, DPA, non-réentraînement)** qui concilie les deux impératifs : secret professionnel absolu et exploitation déléguée sans ressource informatique interne. Le risque d'interruption passe en 🔴 Rouge (continuité de service) et une clause de réversibilité des données est ajoutée en question ouverte (§6).

---

## 2. Besoin métier et contexte (1 paragraphe)

**Demande exprimée par le client :**  
> *« On rédige beaucoup de courriers types (mise en demeure, transmission dossier). On voudrait un assistant pour aller plus vite, et aussi pour retrouver les bonnes jurisprudences en 30 secondes au lieu de 30 minutes. »*

**Besoin réel reformulé :**  
Le cabinet fait face à une perte de temps quotidienne liée à l'éparpillement de son capital documentaire (recherche artisanale et mémoire orale pour retrouver des décisions passées, réécriture manuelle de courriers répétitifs). Le besoin réel est double : d'une part, **retrouver instantanément les décisions obtenues par le cabinet** pour étayer rapidement les dossiers sans dépendre du souvenir d'un confrère ; d'autre part, **éliminer la saisie redondante sur les actes récurrents** (recouvrement, baux commerciaux) tout en sécurisant la cohérence rédactionnelle.

**Contraintes révélées en entretien :**  
- **Déontologie & Secret professionnel :** Exigence absolue de confidentialité (art. 66-5, risque disciplinaire personnel devant le Barreau), exclusion stricte des hallucinations (citations 100 % vérifiables) et validation finale humaine obligatoire par l'avocat signataire.
- **Budget :** Enveloppe fermée de 15 000 € pour la mise en place (Build), suivie d'un coût récurrent de quelques centaines d'euros par mois au maximum (Run).
- **Échéance :** Pas d'urgence calendaire absolue ; le client privilégie formellement la fiabilité et la robustesse juridique à la rapidité de livraison (*« je préfère quelque chose de fiable dans six mois que risqué dans un mois »*).
- **Équipe & Périmètre :** Usage strictement interne (12 avocats et leurs assistantes, aucun accès client externe) ciblé en priorité sur les contentieux récurrents et standardisés (recouvrement et baux commerciaux, exclusion du droit de la famille).
- **Capacité d'exploitation informatique (Imprévu 14h30) :** Arrêt du prestataire informatique au 31 décembre. Absence totale de compétences informatiques internes pour maintenir un serveur physique ; nécessité d'une exploitation déléguée à un tiers infogéré souverain avec SLA garanti.

---

## 3. Données

| Donnée | Existante / à acquérir | Volume, qualité estimée | Personnelle ? |
|---|---|---|---|
| **Registre des décisions internes** | Existante (fournie dans `data/cas_A_registre_decisions_sample.csv`) | ~2 000 entrées sur 15 ans. Très bonne qualité de métadonnées (colonnes identifiant, date, matière, juridiction TJ Bordeaux, issue). | Non (métadonnées d'affaires et de procédure). |
| **Fichiers textuels des décisions** | Existante | ~2 000 documents (PDF natifs, Word, scans anciens). Qualité hétérogène : PDF/Word récents très exploitables, scans papier plus anciens nécessitant une extraction de texte (OCR). | Oui : noms des parties, adresses, éléments de contentieux (PII couvertes par secret pro). |
| **Modèles de courriers types** | Existante | Quelques dizaines de modèles (dossier partagé 2019 + versions locales avocats). Qualité moyenne : disparité des versions et risque d'obsolescence juridique. | Non dans les gabarits types, mais données réelles dans les anciens courriers réutilisés. |
| **Historique des courriers rédigés** | Existante | Plusieurs milliers de courriers Word archivés par dossier client. Bonne qualité formelle mais forte dispersion sur disques locaux. | Oui : identité des clients, débiteurs, montants, faits litigieux. |
| **Jurisprudence nationale publique** | Existante (externe) | Accessible via abonnement commercial existant du cabinet. Qualité excellente et à jour. | Non. Hors périmètre d'ingestion de la solution (déjà couvert). |
| **Données d'entraînement labellisées** | À acquérir (non requises) | Aucune donnée annotée pour entraînement ML supervisé. RAG retenu : pas d'annotation lourde requise. | Sans objet. |

**Constats de qualité observés sur l'extrait réel (`cas_A_registre_decisions_sample.csv`) :**
1. **Excellente structuration des métadonnées** : les décisions disposent déjà d'un identifiant unique (`DEC-xxxx`), d'une date normée (`YYYY-MM-DD`), d'une matière juridique claire (*recouvrement, bail commercial, famille, prud'hommes, droit des sociétés*), d'une juridiction (*TJ Bordeaux*) et d'une issue (*favorable, défavorable, transaction*).
2. **Priorisation facilitée** : l'extrait confirme la prédominance des contentieux cibles (recouvrement et baux commerciaux représentent plus de 50 % des affaires), ce qui valide le périmètre initial restreint convenu avec Maître Devalle.

---

## 4. Risques et conformité

**Usage réel (2-3 lignes) :**  
L'assistant est utilisé **exclusivement en interne** par les assistantes juridiques (pour préparer les projets de courriers) et les 12 avocats (pour retrouver les décisions passées du cabinet et sourcer leurs arguments). L'outil propose des brouillons et des extraits référencés : **il ne prend aucune décision, n'envoie aucun acte et ne conseille aucun client directement**. L'avocat conserve l'obligation déontologique de relire, corriger, valider et signer chaque acte.

**Qualification AI Act :**  
- **Niveau retenu : Système sans obligation spécifique (hors pratiques interdites et hors haut risque).**  
- **Justification raisonnée :** L'Annexe III point 8 (administration de la justice) ne vise que les systèmes d'IA utilisés par une **autorité judiciaire** pour assister l'interprétation des faits ou du droit, ce qui n'est pas le cas d'un cabinet libéral privé. L'outil n'interagit pas non plus avec les justiciables ou le grand public (l'article 50 sur les obligations de transparence des chatbots grand public ne s'applique donc pas). Enfin, le système n'effectue aucun profilage d'individus.  
- **Condition de bascule vers le Haut Risque ou Transparence :** Le système basculerait sous l'article 50 (transparence) si l'assistant était déployé sur le site web du cabinet pour dialoguer directement avec les clients (projet d'associé expressément écarté), ou sous l'Annexe III s'il était utilisé pour évaluer la performance individuelle des collaborateurs ou automatiser des décisions juridictionnelles.

**RGPD :**  
- **Base légale retenue et justifiée :** **Intérêt légitime** (art. 6 §1 f du RGPD) pour l'amélioration de l'organisation interne du cabinet et l'aide à la gestion documentaire de ses dossiers, combiné à l'**exécution du mandat/contrat de prestation juridique** (art. 6 §1 b) pour le traitement des pièces des dossiers confiés par les clients.  
- **Profilage :** Aucun profilage ni notation d'individus ou de salariés n'est réalisé par le système.  
- **Article 22 du RGPD (décision exclusivement automatisée produisant des effets juridiques) :** **Non applicable**. Deux conditions cumulatives font défaut : l'avocat relit et valide obligatoirement chaque acte (décision non exclusivement automatisée, boucle humaine obligatoire) et l'outil n'émet aucun acte juridique autonome.

### Tableau des risques (éthique, métier, conformité)

| Risque (éthique, métier, conformité) | 🔴/🟠/🟡 | Obligation ou raison | Traitement dans l'architecture |
|---|---|---|---|
| **Violation du secret professionnel de l'avocat** | 🔴 Rouge | Obligation d'ordre public (loi du 31/12/1971 art. 66-5 + déontologie Barreau). Sanctions disciplinaires et pénales directes. | **Hébergement souverain étanche certifié SecNumCloud** (ex. OVHcloud/Scaleway) ; contrat DPA interdisant la réutilisation des données ; chiffrement des données au repos et en transit. |
| **Interruption de service & Perte de maintenance informatique (Imprévu 14h30)** | 🔴 Rouge *(passe de 🟠 à 🔴)* | Rupture du contrat du prestataire IT au 31/12 ; absence d'administrateur système interne au cabinet pour gérer un serveur local. | **Bascule de l'on-premise vers un service managé souverain (PaaS/SaaS sécurisé)** avec support technique inclus, sauvegardes automatiques et engagement de réversibilité complète. |
| **Hallucination juridique ou fausse jurisprudence** | 🔴 Rouge | Responsabilité civile professionnelle (RCP) de l'avocat engagée ; risque de sanctions judiciaires pour fausse citation (jurisprudence inventée). | **Architecture RAG avec ancrage strict (Grounding)** : le modèle n'a pas le droit de citer une source hors du corpus injecté ; chaque citation génère un lien direct et cliquable vers le PDF d'origine. |
| **Fuite de données personnelles de clients/justiciables (RGPD)** | 🔴 Rouge | Art. 32 RGPD (sécurité des traitements de données sensibles et judiciaires). | Module de **pseudonymisation / masquage automatique des PII** (noms, adresses, coordonnées bancaires) avant transmission au moteur sémantique. |
| **Obsolescence de la règle de droit citée** | 🟠 Orange | Risque d'erreur de conseil si une décision interne de 2012 applique un texte abrogé. | Filtrage temporel dans les métadonnées et alerte visuelle de date dans l'interface invitant l'avocat à vérifier la validité actuelle sur sa base en ligne. |
| **Dépendance ou sur-confiance des assistantes (automation bias)** | 🟡 Jaune | Risque de validation machinale d'un courrier sans relecture approfondie. | Garde-fou ergonomique imposant une étape explicite de relecture et signature personnelle de l'avocat responsable. |

### Sécurité du modèle — Menaces et robustesse (selon exposition de l'architecture)

*L'architecture étant un outil interne accessible uniquement aux 12 avocats et assistantes via authentification, la surface d'attaque est circonscrite (pas d'API publique ouverte).*

| Menace | Plausibilité sur CE cas | Mitigation proposée | Risque résiduel |
|---|---|---|---|
| **Injection indirecte de prompt (Indirect Prompt Injection)** | 🟠 Plausible (via pièces adverses indexées) | Des documents externes ou courriers de parties adverses numérisés et indexés pourraient contenir des instructions malveillantes dissimulées visant à fausser la réponse du modèle. | **Séparation stricte instructions / données** dans le prompt système, filtrage/assainissement textuel (sanitization) des documents ingérés avant vectorisation. | Document adverse très sophistiqué altérant la forme du résumé sans toutefois court-circuiter la relecture humaine. |
| **Fuite de données confidentielles via requêtes (Data Leakage)** | 🟠 Plausible (en cas d'usage d'API cloud non étanche) | Risque de réutilisation des courriers ou requêtes pour réentraîner un modèle externe public. | **Modèle open-source souverain hébergé localement ou sur infrastructure cloud qualifiée SecNumCloud** avec engagement contractuel de non-rétention des données. | Compromission de l'infrastructure réseau locale du cabinet (géré par PRA/antivirus). |
| **Empoisonnement du jeu de données (Data Poisoning)** | 🟡 Faible | Le corpus de jurisprudence est un fonds fermé validé par le cabinet (seules les décisions réelles du cabinet sont intégrées). | Processus d'ingestion sécurisé : validation des nouveaux documents et contrôle d'accès en écriture au registre documentaire. | Erreur humaine d'enregistrement d'une mauvaise décision dans le registre. |
| **Attaque contradictoire (Adversarial Examples) / Evasion** | ⚪ Sans objet | Pas d'attaquant externe cherchant à classifier une entrée à la volée. Système d'aide documentaire interne. | Écarté : pas d'exposition d'inférence publique. | Aucun. |

---

## 5. Architecture cible et sobriété — mini-cours `05`
_Renvoi vers `schema_archi_cible.md` pour le diagramme complet Mermaid (composants : Ingestion/OCR, Base vectorielle & métadonnées, Module de pseudonymisation, Moteur RAG & LLM souverain, Interface métier / plugin Word)._

**Sobriété argumentée (LLM retenu ou refusé) :**  
Pour ce projet, **un modèle propriétaire grand public américain (ex. OpenAI GPT-4) est formellement refusé** en raison de l'incompatibilité avec le secret professionnel de l'avocat, du coût récurrent imprévisible au token et de son empreinte écologique démesurée.  
L'auto-hébergement sur le serveur interne du cabinet étant devenu intenable avec le départ annoncé du prestataire informatique, nous retenons un **service souverain managé en France (SecNumCloud)** exécutant un **modèle compact open-source spécialisé (Mistral 7B ou Llama 8B)**, couplé à une base vectorielle légère et à un filtrage préalable sur métadonnées. Ce choix concilie secret professionnel absolu (DPA strict, 0 rétention), exploitation sans compétence technique interne, respect du budget initial de 15 000 € et récurrent maîtrisé à ~250 €/mois.

---

## 6. Indicateurs, seuils, questions ouvertes — mini-cours `03`

| Indicateur | Cible | Seuil d'acceptabilité | Comment on le mesure |
|---|---|---|---|
| **Gain de temps quotidien par avocat** | **1 heure / jour / avocat** (soit 12 h/jour cabinet) | ≥ 45 minutes / jour / avocat | Enquête déclarative mensuelle + chronométrage comparatif sur la préparation des dossiers types. |
| **Temps de recherche d'une décision interne** | **< 1 minute** (vs 30 minutes actuellement) | < 3 minutes | Horodatage logs système entre la soumission de la requête et la consultation de la décision pertinente. |
| **Précision et fidélité des sources citées** | **100 % des sources vérifiables et exactes** | 100 % (0 tolérance d'hallucination) | Audit aléatoire par les associés sur 50 requêtes mensuelles : conformité du lien vers le PDF d'origine. |
| **Taux d'adoption par les équipes** | **> 85 % des courriers cibles pré-rédigés via l'outil** | ≥ 70 % après 3 mois | Statistiques d'utilisation : volume mensuel de courriers initiés via l'assistant rapporté au volume total. |
| **Incidents de secret professionnel / fuites** | **0 incident** | **0 incident (seuil absolu)** | Audit continu des flux de données et journalisation des accès. |

### Prochaines étapes (3 jalons clés)
1. **Mois 1-2 : POC ciblé sur un corpus restreint** (Recouvrement & baux commerciaux) sur 200 décisions avec test d'ingestion OCR et validation de l'interface de recherche.
2. **Mois 3-4 : Intégration du module de génération de courriers et sécurisation** (pseudonymisation, ancrage strict RAG, déploiement sur infrastructure souveraine).
3. **Mois 5-6 : Phase pilote en cabinet** avec 3 avocats et les assistantes, ajustements ergonomiques, formation déontologique et déploiement général aux 12 avocats.

### Questions ouvertes restant à clarifier avec le client (reprises de `notes_entretien.md` §3)
1. **Conditions de réversibilité et restitution des données (Impact direct Imprévu IT)** : Quelles sont les clauses contractuelles de réversibilité exigées par le cabinet pour garantir que l'ensemble des index vectoriels, métadonnées et historiques de courriers puissent être récupérés à tout moment sous format standard ouvert en cas de changement de prestataire cloud ou de reprise en main par un nouveau mainteneur informatique ?
2. **Qualité exacte de l'OCR sur les archives anciennes (scans papier)** : Quel est le pourcentage exact de scans papier non lisibles, et faut-il prévoir une prestation de numérisation/OCRisation professionnelle dans les 15 000 € ou se concentrer dans un premier temps sur les 5 dernières années déjà numérisées ?
3. **Modalités de transition avant le 31 décembre** : Le prestataire sortant peut-il fournir d'ici son départ un export propre et consolidé de l'arborescence du serveur local et du registre des décisions vers le nouvel espace souverain sécurisé ?
