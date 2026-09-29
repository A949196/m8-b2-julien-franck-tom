# Document de cadrage — Cabinet Maître Devalle (Bordeaux)

## 1. Synthèse exécutive
Le cabinet perd du temps à retrouver ses propres décisions (environ 10 recherches par jour, estimées à 30 minutes) et à réécrire ses courriers types (environ 15 par jour). Nous proposons **une recherche dans le texte des décisions du cabinet, avec les filtres du registre, et une bibliothèque de courriers types à jour**, hébergées en France par un service maintenu. Aucun outil ne rédige de texte tout seul (IA générative) : chaque résultat renvoie à un document réel que l'avocat vérifie.
Indicateurs clés : recherche de 30 min à moins de 5 min (objectif : moins d'1 min) ; **0 décision inventée** ; environ 25 min gagnées par avocat et par jour (l'objectif d'1 h n'est pas démontré) ; budget visé 15 000 €.

> **Imprévu client — ce que ça change** : votre prestataire informatique s'arrête au 31 décembre, et le serveur du cabinet n'aura plus d'administrateur. L'outil ne s'appuie plus sur ce serveur mais sur un hébergement géré en France ; un nouveau risque 🔴 apparaît ; le travail technique ne démarre qu'une fois les documents sécurisés ; un indicateur de sauvegarde est ajouté.

## 2. Besoin métier et contexte
**Demande exprimée** : « un assistant pour aller plus vite, et retrouver les bonnes jurisprudences en 30 secondes au lieu de 30 minutes ».
**Besoin réel** : le savoir du cabinet est difficile à retrouver et à réutiliser. Deux besoins :
1. **Retrouver vite, avec la preuve, les décisions que le cabinet a lui-même obtenues** (environ 2 000 en quinze ans, cherchées par nom de fichier ou en demandant à un collègue). La jurisprudence publique est déjà couverte par votre abonnement. Environ 10 recherches par jour à 30 min (estimation) : environ 5 h par jour pour tout le cabinet.
2. **Ne plus réécrire les courriers types à partir de vieux courriers** : environ 15 par jour, copiés-collés depuis les postes, le dossier « modèles » datant de 2019.

Priorité : Maître Devalle dit que « c'est surtout la recherche qui prend du temps ». Nous proposons de commencer par elle, à valider avec vous.
**Contraintes** : aucune erreur tolérée (décision inventée ou fuite = responsabilité personnelle devant le Barreau) ; tout résultat vérifiable ; pas de « cloud américain », hébergeur français envisageable ; 15 000 € au démarrage puis quelques centaines d'euros par mois ; « du fiable dans six mois plutôt que du risqué dans un mois » ; explications sans jargon ; **prestataire informatique parti au 31 décembre, remplaçant non choisi**.

## 3. Données

| Donnée | Existante / à acquérir | Volume, qualité estimée | Personnelle ? |
|---|---|---|---|
| Décisions du cabinet (serveur) | Existante | Environ 2 000 sur 15 ans, PDF ou Word ; anciens scans peu lisibles, part inconnue ; aucune décision reçue, qualité non vérifiée | Oui : clients, parties adverses, salariés, enfants ; non anonymisées |
| Registre des décisions (tenu par une assistante) | Existante | Numéro, date, matière, juridiction, issue, fichier ; extrait de 19 lignes ; couverture et mise à jour inconnues | Aucun nom vu sur l'extrait |
| Courriers archivés + dossier « modèles » 2019 | Existante | Des milliers ; aucun exemple reçu ; modèles obsolètes | Probable, non confirmé |
| Jurisprudence publique (abonnement) | Existante | Anonymisée, déjà couverte : **hors périmètre proposé** | Non |
| Texte lisible des anciens scans | **À acquérir** | Reconnaissance automatique de texte (OCR) ; coût selon la part de scans | Oui |
| Contenu de chaque décision (point de droit, mots-clés) | **À acquérir** | Absent du registre | Oui |
| Courriers types à jour | **À acquérir** | À choisir et valider par un avocat | À vérifier |
| **Sauvegarde vérifiée et accès administrateur du serveur** | **À acquérir** (imprévu) | État de la sauvegarde et détenteur des accès inconnus | Oui (mêmes documents) |
| Temps de recherche mesuré | **À acquérir** | Les « 30 min » sont une estimation | Non |

**Étiquettes existantes** : le registre donne matière, juridiction et issue : il permet de filtrer, pas de retrouver une décision par son contenu.
**Constats sur l'extrait (19 lignes, comptées à la main)** : recouvrement 8, bail commercial 4, famille 4, prud'hommes 2, sociétés 1 ; extrait non représentatif (identifiants consécutifs, dates 2020-2025 pour quinze ans annoncés) ; « TJ Bordeaux » sur toutes les lignes, y compris deux affaires prud'homales (à vérifier) ; noms de fichiers non parlants (`decision_1004.pdf`).

## 4. Risques et conformité

**Usage réel** : avocats et assistantes retrouvent une décision du cabinet ou préparent un brouillon de courrier. La sortie (liste de documents ou brouillon) ne déclenche aucune action : l'avocat relit, corrige, signe, et peut l'écarter. Qui vérifie une décision retrouvée n'a pas été précisé (§6).

**Qualification AI Act** (règlement européen sur l'IA) : aucune pratique interdite (art. 5). **Probablement pas haut risque** (art. 6) : pas un produit réglementé (Annexe I) ; le cas « justice » de l'Annexe III vise l'usage par ou pour une autorité judiciaire, ce qu'un cabinet n'est pas ; le cas « emploi » ne s'applique pas tant que l'outil n'évalue pas les salariés (à reconfirmer sur EUR-Lex). Si l'outil dialogue, l'utilisateur doit savoir qu'il parle à une IA (art. 50) et les utilisateurs sont formés (art. 4). **Bascule vers haut risque** si l'outil est utilisé par ou pour un tribunal ou une instance de règlement de litiges, ou pour évaluer ou surveiller le travail des avocats ou assistantes. L'imprévu ne change pas cette qualification.

**RGPD** (protection des données personnelles) : **base légale proposée : intérêt légitime** (art. 6-1-f) : le cabinet retrouve dans ses archives des documents qu'il détient déjà pour exercer son métier. Le consentement est impossible à recueillir auprès des parties adverses, et le contrat lie chaque client à son dossier, pas à une recherche transversale. Conditions : peser l'intérêt du cabinet face aux droits des personnes (dont les enfants), informer les clients, prévoir un droit d'opposition, ne traiter que le nécessaire, fixer une durée de conservation ; réutiliser un dossier pour un autre usage est une finalité nouvelle, compatibilité à vérifier. **Données sensibles** (art. 9) : possibles dans les affaires familiales ; l'exception pour l'exercice ou la défense d'un droit en justice est à valider avec le DPO ou le conseil du cabinet. **Profilage** : aucun prévu. **Art. 22** : non applicable (décision non exclusivement automatisée, aucun effet juridique) ; une relecture faite « pour la forme » changerait l'analyse. **Nouveau avec l'imprévu** : l'hébergeur devient sous-traitant des données : un contrat est nécessaire, à faire valider par le DPO.

| Risque | 🔴/🟠/🟡 | Obligation ou raison | Traitement dans l'archi |
|---|---|---|---|
| Décision ou jurisprudence inventée | 🔴 | Responsabilité professionnelle ; « tout doit être vérifiable » | Seuls des documents réels sont renvoyés, avec lien vers l'original ; vérification par l'avocat |
| Fuite de données couvertes par le secret professionnel | 🔴 | Obligation déontologique | Hébergement en France, accès authentifié, journal des consultations |
| **Serveur du cabinet sans administrateur dès le 1er janvier** (sauvegardes, mises à jour de sécurité, droits d'accès, accès du prestataire sortant) | 🔴 | Secret professionnel ; sécurité des données (RGPD) | Outil hébergé par un service maintenu ; serveur jamais ouvert à l'extérieur ; sauvegarde testée et accès du prestataire sortant révoqués avant le 31/12 |
| Données de tiers et de mineurs (affaires familiales) | 🔴 | RGPD : minimisation, art. 9 | Copie limitée aux documents utiles, chiffrée, en France ; matière « famille » à part si le DPO l'exige |
| Accès élargi : un avocat peut ouvrir les dossiers d'un autre | 🟠 | Secret professionnel | Journal des accès ; droits par dossier à trancher (§6) |
| Anciens scans mal lus : décision existante non retrouvée | 🟠 | Fiabilité | « Aucun résultat » ≠ « aucune décision » ; contrôle de lisibilité |
| Courrier contenant des informations d'un autre client | 🟠 | Secret professionnel | Modèles neutres à jour, relecture obligatoire |

**Tension à arbitrer** : copier les décisions chez un hébergeur français crée une seconde copie, mais laisser l'outil dépendre d'un serveur sans administrateur est plus risqué. Nous recommandons la copie, à confirmer avec vous (§6).

**Sécurité du modèle** — exposition : outil interne, utilisateurs identifiés, documents non publics, aucune interface ouverte à des tiers.

| Menace | Plausibilité sur CE cas | Mitigation proposée | Risque résiduel |
|---|---|---|---|
| **Serveur non mis à jour et sans surveillance** | 🟠 ; 🔴 dès le 1er janvier si rien n'est fait | Reprise par un prestataire ou hébergement géré ; sauvegarde testée ; accès du prestataire sortant révoqués, mots de passe changés | Période sans prestataire ; compte oublié |
| Fuite de documents vers l'extérieur ou un utilisateur non autorisé | 🟠 Documents très sensibles, accès ouvert entre avocats | Authentification, limitation des requêtes, journal des consultations | Personne autorisée qui copie volontairement |
| Consigne cachée dans un document (injection indirecte) | 🟡 seulement si un modèle de langage lit des documents de tiers | Contenu traité comme donnée ; sortie limitée à citer et lier. **Sans objet sans modèle de langage** | Consigne subtile non détectée |
| Faux document ajouté à la base | 🟡 | Ajout réservé au personnel habilité, journalisé | Erreur ou malveillance interne |
| Entrée trompeuse, vol de modèle | ⚪ Écartées : aucun tiers ne soumet d'entrée, aucun modèle propre exposé | — | — |

## 5. Architecture cible et sobriété
Schéma : `schema_archi_cible.md`. Six briques : le serveur du cabinet (source), une copie chiffrée avec préparation des scans (reconnaissance de texte), un index de recherche, une interface pour avocats et assistantes, la bibliothèque de courriers types, un journal des consultations. **Depuis l'imprévu**, l'index, l'interface, les courriers et le journal sont hébergés en France par un service dont la maintenance (mises à jour, sauvegardes) est incluse au contrat, et non sur le serveur du cabinet. Le serveur n'est jamais ouvert sur Internet : il envoie une copie à sens unique.
**Calendrier** : aucun branchement technique avant le 31 décembre. D'ici là : sauvegarde testée, accès et documentation récupérés auprès du prestataire sortant.
**Bascule** : si le cabinet déplace tout son serveur vers un stockage hébergé en France avec son futur prestataire, la copie disparaît et l'outil lit directement ce stockage.

**Modèle de langage (IA qui rédige) : refusé à ce stade.** Le besoin est de retrouver des documents réels, pas d'en générer, et une décision inventée est le risque exclu. Une recherche dans le texte suffit pour environ 2 000 décisions, tient dans le budget et, sans IA à maintenir, convient à un cabinet qui n'a plus d'informaticien. **Bascule** : si la recherche par mots ne retrouve pas la bonne décision assez souvent (§6), on envisagera une aide par un modèle hébergé en France, sans citation produite (grille C4).
**Écarté** : assistant en langage naturel (RAG), agents autonomes, modèle entraîné sur vos décisions, service hors de France.

## 6. Indicateurs, seuils, questions ouvertes

| Indicateur | Cible | Seuil d'acceptabilité | Comment on le mesure |
|---|---|---|---|
| Temps pour retrouver une décision | 30 min (estimation) → moins d'1 min ; soit environ 25 min gagnées par avocat et par jour | 5 min en moyenne (proposé), soit environ 20 min gagnées | Chronométrage 2 semaines avant, puis journal des consultations |
| Bonne décision dans les 5 premiers résultats | ≥ 90 % (proposé) | ≥ 80 % (proposé) : un échec se rattrape par le registre ou un collègue | Test sur 30 recherches réelles |
| Décisions inventées ou sans document source | 0 | 0, non négociable (responsabilité personnelle) | Test des 30 recherches, puis contrôle régulier |
| **Sauvegarde du serveur** (imprévu) | 1 restauration réussie avant le 31/12, puis un test par mois (proposé) | Jamais sans sauvegarde vérifiée | Compte rendu de test |
| Coût | Mise en place ≤ 15 000 € ; abonnement « quelques centaines d'euros par mois », hébergement maintenu inclus | Plafond mensuel à chiffrer | Devis de l'hébergeur et des prestataires |

Les valeurs « proposé » viennent de nous, pas d'un chiffre donné en entretien. Le seuil sur les erreurs se justifie par leur coût : une décision non retrouvée se rattrape, une décision inventée engage la responsabilité personnelle. L'objectif d'1 h gagnée par avocat et par jour n'est pas démontré : la recherche seule en libère environ 25 min ; le reste dépend des courriers, dont le temps est inconnu.

**Prochaines étapes** : (1) **avant le 31/12** : faire tester une sauvegarde, récupérer accès administrateur et documentation, puis révoquer les accès du prestataire sortant ; (2) choisir avec le futur prestataire l'hébergement géré en France et l'option pour les documents (copie ou déplacement), et mesurer les temps actuels ; (3) faire valider par le DPO la base légale, les données sensibles et le contrat d'hébergement.

**Questions ouvertes**
- **Serveur** : existe-t-il une sauvegarde, testée ? Qui détient les accès administrateur ? Le contrat qui s'arrête couvre-t-il aussi le logiciel de gestion, Microsoft 365 et les postes ?
- **Nouveau prestataire** : quand sera-t-il choisi ? Acceptez-vous une copie chiffrée chez un hébergeur français, ou préférez-vous y déplacer tout le serveur ?
- **Données** : le registre est-il complet et à jour ? Pourquoi la recherche prend-elle 30 min malgré lui ? Quelle part de scans ? Pourquoi les décisions n'ont-elles pas pu être transmises ?
- **Mesures** : minutes par courrier ? Les 30 min de recherche sont-elles mesurées ?
- **Accès** : assistantes, droits par dossier ou par matière (famille), journal des accès ?
- **SI** : nom et intégration du logiciel de gestion ; Microsoft 365 est-il compatible avec le refus du « cloud américain » ? garanties attendues d'un hébergeur « sérieux » ?
- **Budget** : que couvrent les 15 000 € ? Le coût du nouveau prestataire est-il hors budget ? Budget ferme ?
- **Priorité** : recherche ou courriers d'abord ? Quel taux de bonnes décisions jugez-vous suffisant ? Quelles données sensibles dans les affaires familiales ? Un outil a-t-il déjà été essayé ?
