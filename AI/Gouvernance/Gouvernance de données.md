Je suis en train d'essayer de me faire une idée sur la gouvernance de données à la CCQ. Il y a déjà un projet en  cours, je te donnerai là ou il en sont et de ce que j'ai compris de ce qu'ils voulaient faire.
Mais avant, je voudrais me faire une idée de ce que l'on attend d'une gouvernance de données dans une organisation, surtout avec les projet d'IA qui arrivent à grand pas.

### Environnements technologiques
1. **Le Cœur ERP & RH (Écosystème SAP) :**
    - **S/4HANA** (sur VM) comme moteur central.
    - **SuccessFactors (SF)** scindé en deux instances : une pour la gestion RH (Core HR) et une dédiée à la paie/reporting (ECP).
    - **SAP BTP & CPI** agissant comme le chef d'orchestre pour l'intégration et le flux de données entre ces modules.
2. **L'Expérience Utilisateur & Services (ServiceNow) :**
    - Portail de services pour les employés de la construction et les employeurs.
    - Outil pivot pour le centre de services (gestion de la relation client/dossiers).
3. **Gestion des Identités (IAM) :**
    - Dualité d'accès : **ADLDS** pour l'interne et **SAP Customer Data Cloud (Gigya)** pour l'externe (B2B/B2C).
4. **Gestion Documentaire :**
    - **OpenText** comme source unique de vérité pour les documents, interconnecté à SAP et ServiceNow.
5. **Dette Technique & Coexistence (Mainframe & .NET) :**
    - Maintien du **Mainframe (IBM z/OS)** pour le back-office historique.
    - Services en ligne legacy en **.NET/C#**.
    - Données "shadow IT" ou locales à migrer (**Access, Excel, IBM i2**).
6. **Infrastructure Data & Pivot (Azure) :**
    - **Azure** sert de plateforme BI (Data Analytics).
    - Rôle critique de **middleware** pour synchroniser les données entre le "Nouveau Monde" (SAP/SNOW) et l'Ancien (Mainframe).
### Comment la gouvernance va nous aider au quotidien
Dans notre environnement hybride complexe (Mainframe, SAP, SaaS), la gouvernance de données ne sera pas une contrainte administrative, mais un levier opérationnel qui agira sur cinq axes précis :
- **Détermination de la criticité :** Elle nous permettra de distinguer formellement les **données sensibles** (ex: salaires, dossiers de santé, secrets industriels) des données publiques. Cela garantit que nos ressources de protection sont investies là où les risques sont les plus élevés.
- **Identification des propriétaires (Data Owners) :** Elle mettra fin à l'incertitude du "qui décide ?". Chaque domaine de données aura un responsable désigné capable d'autoriser les accès et de valider les règles de gestion lors des migrations vers SAP ou le futur MDM.
- **Catégorisation et nomenclature :** En classifiant les données par familles (RH, Employeurs, Financier), nous créons un langage commun. Un "employeur" aura la même définition dans ServiceNow que dans le Mainframe, facilitant ainsi la réconciliation des dossiers.
- **Sécurisation uniforme et "Privacy by Design" :** La gouvernance définit les règles de protection qui seront appliquées partout. Pour nos **projets IA**, elle garantit l'anonymisation automatique des PII (données personnelles), nous permettant d'innover sans risquer de fuites de données sensibles.
- **Fiabilité de la décision (Single Source of Truth) :** En définissant quel système est "maître" (ex: SAP pour le RH, Mainframe pour l'historique), elle élimine les doublons et les écarts de chiffres dans les rapports BI, redonnant une confiance totale aux décideurs.
### Implémentation et Rework technologique minimal
L'implémentation ne sera pas qu'une affaire de politiques, elle nécessitera des ajustements ciblés sur nos plateformes pour aligner la technique sur la stratégie.
#### Stratégie d'implémentation :
L'approche sera progressive : d'abord définir les **propriétaires (Data Owners)** et les **règles de qualité**, puis utiliser le **MDM** comme bras technologique pour fusionner les données du Mainframe et de SAP.
#### Rework minimal nécessaire (Ajustements techniques) :
Pour que la gouvernance soit effective, les modifications suivantes sont à prévoir :
- **Écosystème SAP (S/4HANA & SF) :**
    - **Ajustement des rôles (PFCG) :** Aligner les accès sur la nouvelle classification (ex: restreindre l’accès aux données critiques).
    - **UI Masking :** Configuration possible pour masquer les champs PII à l'écran pour les utilisateurs non autorisés.
- **ServiceNow :**
    - **Révision des ACL (Access Control Lists) :** S'assurer que les dossiers clients ne sont consultables que selon les règles de propriété définies.
    - **Nettoyage des formulaires :** Supprimer ou sécuriser les champs de saisie libre qui collectent des données sensibles sans contrôle.
- **Plateforme BI (Azure) :**
    - **Sécurité au niveau des lignes (Row-Level Security) :** Mise en place d'un filtrage dynamique pour que les rapports n'affichent que les données autorisées selon le profil utilisateur.
    - **Data Lineage :** Configuration des outils Azure pour tracer le flux de la donnée du Mainframe jusqu'au rapport final.
- **Systèmes Legacy (Mainframe & .NET) :**
    - Le rework sera ici limité à la **sécurisation des flux de sortie**. Au lieu de modifier le code obsolète, on s'assurera que les transferts vers Azure ou SAP filtrent ou chiffrent les données sensibles dès leur sortie du z/OS.
### Comment le projet va démarrer?

#### Étape 1 : Cartographie et Classification automatique (Le "Scan")

Purview va parcourir votre EDO.

- **Action :** Configurez des "Classifiers" (règles) pour détecter automatiquement les **PII** (numéros d'assurance sociale, noms, adresses).
- **Résultat :** Vous obtenez immédiatement une vision de la **criticité** de vos données sans intervention manuelle.

#### Étape 2 : Définition des Propriétaires (Glossaire Métier)

Une fois les données identifiées dans Purview, il faut leur donner un sens métier.

- **Action :** Dans le _Business Glossary_ de Purview, créez les termes "Salarié", "Employeur", "Cotisation".
- **Lien :** Assignez à chaque terme un **Data Owner** (ex: le Directeur RH pour les données provenant de SuccessFactors). C'est ici que vous formalisez la **propriété de la donnée**.

#### Étape 3 : Catégorisation et Lignage (Lineage)

C'est la force de Purview.

- **Action :** Visualisez d'où vient la donnée (ex: du Mainframe vers l'EDO via Azure Data Factory).
- **Résultat :** Cela vous aide à voir si une donnée sensible "fuit" vers un environnement BI moins sécurisé. Vous **catégorisez** ainsi vos actifs par domaine (RH, Finance, Construction).

#### Étape 4 : Sécurisation et Préparation du MDM

Purview ne sécurise pas directement SAP ou ServiceNow, mais il dicte les règles.

- **Action :** Utilisez les rapports de conformité de Purview pour identifier où un **rework** est nécessaire (ex: "Je vois des données de paie en clair dans l'EDO, il faut appliquer du masquage dans la source SAP via PFCG").
- **Vers le MDM :** Purview servira de "guide de nettoyage". Avant d'envoyer les données dans le MDM, Purview vous dira lesquelles sont de mauvaise qualité ou mal classifiées dans l'EDO.

#### Pourquoi commencer par le BI (EDO) est malin :

1. **Visibilité rapide :** Vous montrez des résultats à la direction en quelques semaines.
2. **Sécurité IA :** Vos projets IA puisent souvent dans l'EDO. En sécurisant l'EDO via Purview, vous sécurisez indirectement vos futurs modèles d'IA.
3. **Moins de risques :** Vous ne risquez pas de ralentir SAP ou le Mainframe pendant la phase de découverte.
