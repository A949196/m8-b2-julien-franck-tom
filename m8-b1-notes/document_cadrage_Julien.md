# Document de cadrage — _ton cas_ (À COMPLÉTER — 3 pages max)

> **Livrable principal.** Lisible par le persona client (pas un dev). Renomme en
> `document_cadrage.md`. Les analyses (besoin, données, risques, KPI) se font
> **directement ici** : pas de fichiers séparés. Le schéma vit dans
> `schema_archi_cible.md`, tes notes dans `notes_entretien.md`.
> **3 pages est un plafond** : phrases courtes, tableaux, pas de remplissage.

## 1. Synthèse exécutive (5-6 lignes — rédigée EN DERNIER)
_Besoin réel + solution proposée (famille, pas la stack) + 2-3 indicateurs clés._

> **Imprévu client (14h30) — ce que ça change** : _1-2 lignes : quelle contrainte
> a bougé, quelles sections tu as mises à jour (données ? risques ? archi ? KPI ?)._

## 2. Besoin métier et contexte

Demande exprimée :
- « On voudrait un assistant pour aller plus vite, et aussi pour retrouver les bonnes jurisprudences en 30 secondes au lieu de 30 minutes. »

Constats issus de l’entretien :
- Les courriers sont préparés par copier-coller depuis des fichiers individuels
- le dossier partagé de modèles « date de 2019 et personne ne le met à jour »
- Pour les décisions internes, les avocats recherchent « par nom de fichier » ou demandent « au collègue qui s’en souvient »
-  Le cabinet dispose d’environ 2 000 décisions sur quinze ans. Maître Devalle attend un gain « d’une heure par jour » par avocat et une recherche en « moins d’une minute », sans « le moindre risque déontologique ». Elle exige que « tout [soit] vérifiable ».

Contraintes issues de l’entretien :

- Les réponses doivent être vérifiables afin d’éviter toute référence à une jurisprudence inexistante.
- L’avocat conserve la responsabilité de l’utilisation d’une décision dans un dossier.
- Les informations des dossiers sont couvertes par le secret professionnel.
- Le cabinet refuse que ses dossiers soient traités dans un cloud américain. Un hébergeur français sous contrat est envisageable.
- Les modèles de courriers partagés sont obsolètes et non maintenus.
- Une partie des décisions anciennes est numérisée sous forme de scans peu lisibles.

Besoin reformulé :
- Le cabinet doit réduire le temps nécessaire à l’identification de jurisprudences pertinentes dans son fonds documentaire, afin de permettre aux avocats de les retrouver en moins d’une minute pour les aider à émettre un avis professionnel tout en préservant leur responsabilité professionnelle. 

## 3. Données

| Donnée | Existante / à acquérir | Volume, qualité estimée | Personnelle ? |
|---|---|---|---|
| Registre des décisions | Existante | Extrait transmis de 20 lignes. Métadonnées structurées: identifiant, date, matière, juridiction, issue et nom du fichier. Formats homogènes et données complètes sur l’extrait. | Non observé dans l’extrait |
| Décisions internes du cabinet | Existante | Environ 2 000 décisions sur 15 ans, en PDF ou Word. Les plus anciennes sont des scans parfois peu lisibles. | Oui, probable |
| Courriers archivés | Existante | Plusieurs milliers dans les dossiers clients. Qualité non évaluée; les modèles partagés datent de 2019 et ne sont pas maintenus. | Oui, probable |
| Base juridique externe | Existante sous abonnement | Consultable par les avocats; droits d’indexation par l’outil à vérifier. | À confirmer |
| Texte extrait des scans | À acquérir / à produire | Une extraction par OCR sera nécessaire pour rendre les scans recherchables. Qualité à mesurer. | Selon le document |

**Constats sur l’extrait transmis.** Le registre est un fichier CSV structuré, avec séparateur `;`. Les 20 lignes fournies sont complètes et couvrent quatre matières juridiques, avec une juridiction unique: le tribunal judiciaire de Bordeaux. Il permet de filtrer et localiser une décision, mais ne contient pas le texte intégral indispensable pour rechercher un raisonnement juridique ou produire une réponse sourcée.

**Questions ouvertes.** Confirmer les droits techniques d’accès aux fichiers du serveur, les droits d’indexation de la base juridique externe, la présence de données personnelles dans les décisions et courriers, et la durée de conservation des requêtes.

## 4. Risques et conformité

**Usage réel.** Les avocats du cabinet utilisent l’outil pour identifier des décisions internes potentiellement pertinentes à un dossier. L’outil affiche des résultats et leurs sources, sans produire de décision juridique ni envoyer de document. L’avocat vérifie chaque décision et choisit seul de l’utiliser dans son avis professionnel.

**Qualification AI Act.** L’usage prévu est un outil interne d’assistance à la recherche documentaire. Il ne correspond pas à un cas de haut risque de l’Annexe III: le cabinet n’est pas une autorité judiciaire et l’outil ne prend aucune décision concernant une personne. Si une interface conversationnelle est retenue, les utilisateurs devront être informés qu’ils utilisent une IA (art. 50). Cette qualification doit être revue si l’outil prend une décision juridique ou est utilisé par une autorité judiciaire.

**RGPD.** Les décisions et courriers sont susceptibles de contenir des données personnelles. La base légale envisagée est l’intérêt légitime du cabinet à améliorer la recherche juridique, sous réserve d’une analyse de mise en balance, de minimisation et d’information des personnes. L’outil ne réalise ni profilage ni décision exclusivement automatisée produisant un effet juridique: l’article 22 ne s’applique pas dans le périmètre prévu.

| Risque | Niveau | Obligation ou raison | Traitement dans l’architecture |
|---|---:|---|---|
| Fuite d’informations couvertes par le secret professionnel | 🔴 | Responsabilité déontologique du cabinet | Hébergement français contractualisé, chiffrement, authentification et droits d’accès |
| Jurisprudence inventée, inexacte ou non pertinente | 🔴 | Responsabilité professionnelle de l’avocat | Source et document d’origine affichés; absence de réponse sans source fiable; revue par l’avocat |
| Accès à un dossier par une personne non autorisée | 🔴 | Confidentialité, secret professionnel, RGPD | Accès par rôle, principe du moindre privilège et journalisation |
| Indisponibilité ou défaut de sécurité du serveur après le 31 décembre | 🔴 | Fin du contrat du prestataire actuel | Hébergement infogéré en France, sauvegardes, supervision et responsabilités définies par contrat |
| Courrier obsolète réutilisé | 🟠 | Dossier de modèles non maintenu depuis 2019 | Courriers exclus de la V1; validation et versionnage avant intégration |
| Indexation non autorisée de la base juridique externe | 🟠 | Respect de la licence de la base | Vérifier les droits d’usage avant toute indexation |
| Réponse prise pour un avis juridique définitif | 🟡 | Risque de mauvaise interprétation | Message d’avertissement et validation humaine obligatoire |

**Sécurité du modèle.** L’exposition principale concerne les documents confidentiels et l’interface interne. Aucune action automatique ni réentraînement à partir des retours utilisateurs n’est prévu dans la V1.

| Menace | Plausibilité sur ce cas | Mitigation proposée | Risque résiduel |
|---|---|---|---|
| Fuite de documents par un utilisateur ou un hébergeur | 🔴 : corpus couvert par le secret professionnel | Hébergement français infogéré, chiffrement, authentification, accès par rôle et journaux | Erreur de paramétrage ou utilisateur interne malveillant | 🟠 si une IA générative est utilisée | Séparer les instructions de l’outil du contenu documentaire; filtrer les documents; aucun droit d’écriture | Réponse dégradée, sans action automatique |
| Extraction par appels massifs | 🟡: outil réservé au cabinet | Accès authentifié, limitation et surveillance des requêtes | Utilisateur interne abusif |
| Empoisonnement des données | 🟡: pas de réentraînement prévu | Validation humaine des documents avant indexation | Document erroné validé à tort |

Les attaques adversariales sur des entrées et le réentraînement empoisonné sont écartés de la V1: l’outil n’analyse pas d’entrées fournies par le public et ne réapprend pas automatiquement.

## 5. Architecture cible et sobriété

Le schéma de [schema_archi_cible.md](schema_archi_cible.md) distingue deux niveaux. Le premier est une recherche documentaire RAG: seules les décisions autorisées sont sélectionnées, normalisées, découpées en passages puis indexées dans une base hébergée en France et administrée par un prestataire avec sauvegardes, mises à jour de sécurité et supervision contractuellement définies. L’avocat authentifié obtient une première sortie composée des passages retrouvés, de leurs références et du document source. Les accès et recherches sont journalisés.

Le second niveau, facultatif, ajoute un modèle de langage. Il rédige une proposGPT-5.6 Terra • 24.2 credits
ition à partir de la question de l’avocat et des passages déjà retrouvés. Il ne produit aucune réponse lorsqu’aucune référence n’est disponible. Dans les deux cas, l’avocat relit et valide la décision avant toute utilisation professionnelle. Les décisions validées peuvent enrichir le corpus après contrôle humain explicite, sans ajout automatique.

Le choix d’un LLM est donc limité à la reformulation d’informations déjà sourcées; il n’est pas nécessaire pour la première sortie de recherche. Cette approche réduit le risque d’invention de jurisprudence et permet une V1 utile même sans génération. La V1 exclut les courriers, dont les modèles sont obsolètes, ainsi que la base juridique externe tant que les droits d’indexation ne sont pas confirmés. Aucun agent avec droit d’écriture ni réentraînement automatique n’est prévu.

La continuité de service après le 31 décembre impose de confier l’exploitation technique à un hébergeur français infogéré ou au futur prestataire informatique. La responsabilité des sauvegardes, mises à jour et incidents doit être définie avant le pilote.

## 6. Indicateurs, seuils, questions ouvertes

| Indicateur | Cible | Seuil d’acceptabilité | Comment on le mesure |
|---|---|---|---|
| Temps de recherche d’une décision interne | < 1 minute | À valider avec le cabinet | Chronométrer des recherches représentatives avant et pendant le pilote |
| Temps gagné par avocat | 1 heure par jour | À définir après mesure initiale et volume quotidien de recherches | Comparer le temps consacré à la recherche avant et après usage |
| Résultats vérifiables | 100 % des résultats affichent une source | 100 % | Contrôler la présence d’une référence et l’accès au document source |
| Jurisprudence inexistante citée | 0 | 0 | Revue humaine des résultats du pilote |
| Décisions pertinentes retrouvées | À mesurer sur un jeu de cas validés | À définir avec les avocats | Les avocats évaluent les résultats de recherche |

**Questions ouvertes.**
- Quel est le nombre moyen de recherches de jurisprudence réalisées chaque jour par avocat ?
- Quelle est la date cible du démonstrateur ?
- Quel budget est prévu pour le démonstrateur ?
- L’infrastructure actuelle permet-elle à un outil d’accéder aux décisions stockées sur le serveur ?
- Les conditions de l’abonnement juridique autorisent-elles l’indexation de son contenu ?
- Quelle durée de conservation faut-il appliquer aux requêtes et aux journaux ?
- Quel référent avocat validera les documents avant leur indexation ?

**Prochaines étapes.**
1. Confirmer l’accès technique aux décisions et sélectionner un échantillon de documents autorisés.
2. Évaluer la qualité des PDF, Word et scans afin de définir leur traitement.
3. Construire un pilote de recherche sourcée, puis le tester sur des cas réels validés par les avocats.

_Prochaines étapes (3) + **questions ouvertes** au client (reprises de `notes_entretien.md` §3)._
