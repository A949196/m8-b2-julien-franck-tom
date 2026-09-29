# Comparaison et convergences des cadrages — Franck, Julien, Tom

> Document de synthèse préparatoire au remplissage du §1 de `dossier_conception.md` (exercice M8-B2).  
> Date : 29/09/2026 — Client : Cabinet Maître Devalle (12 avocats, Bordeaux).

---

## 1. Tableau comparatif synthétique des 3 visions

| Dimension | Franck (`document_cadrage_Franck.md`) | Julien (`document_cadrage_Julien.md`) | Tom (`document_cadrage_Tom.md`) |
|---|---|---|---|
| **Posture technologique** | **RAG complet avec SLM souverain** (Mistral 7B / Llama 8B) dès la V1. | **Approche modulaire à 2 niveaux** : niveau 1 recherche documentaire pure, niveau 2 LLM/SLM facultatif. | **Sobriété radicale (Zéro GenAI en V1)** : moteur plein texte + métadonnées, gabarits statiques. |
| **Périmètre fonctionnel V1** | **Recherche + Rédaction assistée** de courriers récurrents (recouvrement, baux). | **Recherche uniquement** en V1. Courriers exclus (fonds de modèles 2019 obsolète). | **Recherche prioritaire** + bibliothèque statique de modèles à jour (aucun courrier généré). |
| **Périmètre des données** | Focus initial sur **recouvrement et baux commerciaux** (~50 % du fonds). Famille exclue. Pseudonymisation active. | Ensemble du registre, mais exclusion des courriers et de la base externe sous licence. | Ensemble du fonds interne (~2 000 décisions), alerte spécifique sur le droit de la famille (art. 9 RGPD). |
| **Gestion de l'imprévu IT (31/12)** | Bascule immédiate vers un **PaaS/SaaS souverain managé (SecNumCloud)** avec SLA et DPA. | **Hébergement infogéré en France** avec contrat de maintenance déléguée. | **Prudence opérationnelle** : arrêt de tout branchement avant audit, sauvegarde testée et révocation des accès sortants avant le 31/12. |
| **KPI de gain de temps** | **1 heure / jour / avocat** (reprise directe de l'attente client). | **1 heure / jour / avocat** (sous réserve de mesure initiale). | **25 minutes / jour / avocat** (calcul critique fondé sur 10 recherches réelles de 30 min / 12 avocats). |

---

## 2. Socle commun (Consensus acquis)

Ces éléments ne font pas débat entre les trois cadrages et constituent les fondations de l'architecture cible :

1. **Confidentialité & Déontologie :** Exclusion totale des clouds américains (soumis au Cloud Act) et des modèles publics fermés (type OpenAI/ChatGPT).
2. **Infrastructure cible :** Hébergement nécessairement localisé en France, infogéré et sous contrat DPA strict (DPA interdisant le réentraînement).
3. **Contrôle humain (Human-in-the-loop) :** Boucle humaine obligatoire ; l'avocat reste seul signataire et responsable déontologique (aucun envoi d'acte autonome).
4. **Qualification AI Act :** Système hors Annexe III (le cabinet n'est pas une autorité judiciaire) et hors pratiques interdites (art. 5).
5. **Base juridique RGPD :** Intérêt légitime (art. 6 §1 f) articulé avec l'exécution du mandat client (art. 6 §1 b), sans application de l'article 22 (pas de décision automatisée).

---

## 3. Les 5 divergences majeures à trancher en groupe

Conformément à la consigne de `ressources/03_Convergence_binome_essentiel.md`, ces divergences ne doivent pas faire l'objet d'un « compromis mou » (tout cumuler) ni d'un simple vote à la majorité 2 contre 1 sans justification.

### ⚡ Divergence 1 : Place de l'IA générative dans l'architecture cible
- **Franck :** RAG complet avec SLM souverain (Mistral 7B / Llama 8B) pour synthétiser les décisions et rédiger les projets d'actes.
- **Julien :** Découplage strict. La V1 est un moteur de recherche indexé ; la couche de génération (LLM) est un second niveau optionnel activé seulement si nécessaire.
- **Tom :** Refus formel de la GenAI en V1. Une recherche plein texte (BM25 / index inversé) avec filtres métadonnées suffit pour 2 000 décisions, supprime le risque d'hallucination (0 tolérance déontologique) et évite de surcharger un cabinet sans informaticien.
- **Enjeu de l'arbitrage :** Risque d'hallucination vs valeur ajoutée perçue par les avocats, coût d'inférence GPU et complexité de maintenance.

### ⚡ Divergence 2 : Périmètre de la V1 (Recherche seule vs Recherche + Courriers)
- **Franck :** Intègre les courriers répétitifs (recouvrement et baux) dès la V1 pour atteindre le ROI et le gain de temps attendu.
- **Julien :** Exclut formellement les courriers en V1 car le fonds de modèles de 2019 est obsolète, non maintenu et source d'erreurs juridiques.
- **Tom :** Priorité absolue à la recherche. Traitement des courriers par une remise à plat organisationnelle (gabarits types validés par un avocat référent), sans automatisation logicielle dans un premier temps.
- **Enjeu de l'arbitrage :** Charge de fiabilisation documentaire préalable vs satisfaction de l'expression de besoin initiale.

### ⚡ Divergence 3 : Données sensibles, droit de la famille et RGPD
- **Franck :** Contourne le problème en éliminant le droit de la famille du périmètre V1 et en intégrant un pipeline de pseudonymisation/masquage des PII.
- **Julien :** Traite l'ensemble sous base légale de l'intérêt légitime avec analyse de mise en balance et minimisation, sans ségrégation explicite de matière.
- **Tom :** Identifie un risque 🔴 spécifique sur les données sensibles (art. 9 RGPD et mineurs dans les contentieux familiaux) et propose soit une séparation étanche des dossiers familiaux, soit une validation formelle par le DPO avant ingestion.
- **Enjeu de l'arbitrage :** Complexité du pipeline d'ingestion (OCR + anonymisation) vs couverture du fonds documentaire.

### ⚡ Divergence 4 : Calendrier et gestion technique de l'imprévu client (départ du prestataire IT)
- **Franck & Julien :** La solution est purement architecturale : basculer vers un cloud souverain infogéré français (PaaS / SecNumCloud) pour ne rien faire peser sur le serveur local.
- **Tom :** Alerte sur la transition immédiate : conditionne tout démarrage technique à des actions d'urgence avant le 31/12 (audit des accès admin, test réel de restauration de sauvegarde, révocation des accès du prestataire sortant).
- **Enjeu de l'arbitrage :** Sécurité opérationnelle immédiate du client vs calendrier de livraison du projet d'assistant.

### ⚡ Divergence 5 : Réalisme du KPI métier (Gain de temps quotidien)
- **Franck & Julien :** Reprennent l'objectif client d'1 heure gagnée par jour par avocat.
- **Tom :** Démontre par le calcul que la recherche seule ne peut faire gagner qu'environ 20 à 25 minutes par avocat par jour (10 recherches quotidiennes au cabinet $\times$ ~25-30 min divisées par 12 avocats), et que promettre 1 heure sans traiter les courriers est intenable.
- **Enjeu de l'arbitrage :** Engagement contractuel réaliste vs promesse commerciale basée sur le ressenti client.

---

## 4. Grille de décision à compléter pour le §1 de `dossier_conception.md`

| Divergence | Positions (qui pensait quoi) | Décision retenue | Pourquoi |
|---|---|---|---|
| **Place de la GenAI (RAG + SLM vs Moteur plein texte)** | **Franck :** RAG + SLM souverain dès V1.<br>**Julien :** Moteur d'abord, LLM en option 2.<br>**Tom :** Moteur plein texte sobre, 0 GenAI en V1. | **RAG avec SLM compact souverain (Mistral 7B / Llama 8B) strictement borné à la synthèse comparative et explicative des extraits retrouvés (Option B).** | **Valeur d'usage métier démontrée :** l'avocat ne veut pas ouvrir et relire 3 décisions intégrales de 15 pages pour chaque question ; le SLM produit une synthèse concise (3-4 phrases) expliquant *pourquoi* les décisions s'appliquent, avec liens cliquables vers les pages et paragraphes exacts.<br>**Garde-fous anti-hallucination :** prompt d'ancrage strict (*grounding*) interdisant toute invention ; si aucun passage ne répond, l'assistant répond formellement « Aucune décision interne ne répond à cette question ».<br>**Maîtrise financière :** l'usage de requêtes courtes d'explication permet de rester sous la barre des 50 à 100 €/mois d'inférence GPU managée. |
| **Périmètre V1 (Courriers types)** | **Franck :** Inclus (recouvrement / baux).<br>**Julien :** Exclus (modèles 2019 obsolètes).<br>**Tom :** Exclus du logiciel (remise à plat organisationnelle). | **Recherche seule en V1. Exclusion des courriers types du périmètre initial.** | **Sécurité juridique & dette documentaire :** les modèles partagés datent de 2019 et ne sont plus maintenus ; automatiser des courriers sur une base obsolète créerait un risque d'erreur de droit immédiat. **Priorité métier exprimée :** Maître Devalle a indiqué que « c'est surtout la recherche qui prend du temps ». Les courriers feront d'abord l'objet d'une remise à niveau humaine des gabarits avant d'envisager une phase 2. |
| **Données sensibles & Famille (art. 9 RGPD)** | **Franck :** Exclure la famille + pseudonymisation.<br>**Julien :** Intérêt légitime global.<br>**Tom :** Alerte art. 9 / mineurs, validation DPO requise. | **Exclusion formelle du droit de la famille du périmètre V1.** Focus exclusif sur les contentieux économiques et patrimoniaux (recouvrement, baux commerciaux, sociétés, etc.) couplé à une pseudonymisation des PII. | **Conformité RGPD et minimisation du risque juridique :** les affaires familiales concentrent des données hautement sensibles (art. 9 RGPD : santé, mœurs, situations familiales conflictuelles) ainsi que des données relatives à des mineurs. L'exclusion de cette matière élimine le risque de traitement illicite sans bloquer le projet en attendant une validation complexe du DPO. De plus, le recouvrement et les baux représentent déjà plus de 50 % du volume de contentieux récurrent. |
| **Prise en compte de l'imprévu IT (31/12)** | **Franck & Julien :** PaaS/SaaS managé FR.<br>**Tom :** Pré-requis bloquants avant le 31/12 (sauvegarde + accès). | **Cible : Cloud souverain managé FR (SecNumCloud) avec intégration de l'alerte opérationnelle de Tom en Jalon 0 impératif.** | **Stratégie en 2 temps :**<br>1. *Jalon 0 (avant le 31/12 — Alerte Tom)* : Test réel d'une restauration complète des sauvegardes, récupération documentée des accès administrateur racine, et révocation formelle des accès du prestataire sortant.<br>2. *Cible pérenne (Franck/Julien)* : Déploiement sur un PaaS/SaaS souverain infogéré (ex. OVHcloud / Scaleway) avec DPA strict et sauvegardes automatiques pour pallier l'absence définitive d'administrateur système interne au cabinet. |
| **Cible du KPI gain de temps** | **Franck & Julien :** 1h / jour / avocat.<br>**Tom :** 20-25 min / jour / avocat (calcul réel). | **Cible réaliste retenue à 20-25 minutes / jour / avocat pour la V1 (recherche seule).** | **Rigueur de calcul et crédibilité professionnelle :** sur la base des ~10 recherches quotidiennes estimées à 30 minutes, le gain maximal théorique est de 5 heures par jour pour l'ensemble du cabinet, soit ~25 min/avocat/jour si la recherche passe à moins d'1 minute. Promettre 1 heure complète sans automatiser les courriers est mathématiquement intenable. L'objectif d'une heure sera réservé comme cible globale si la V2 intègre l'assistance aux courriers. |
