[[Histoire de l’IA Générative - De Toronto à ChatGPT]]
[[Projets IA]]
### Rôles et responsabilités
- **Architecte d'entreprise :** Valide la faisabilité et l'éthique des projets de Niveau 2, 3 et 4.
- **Architecte Solution :** Définit si l'usage d'un SLM local est préférable à un LLM SaaS selon la sensibilité des données.
- **L'Opérateur (Zena/IT Ops) :** Responsable de la disponibilité des déclencheurs et de la surveillance des coûts de tokens.
### La couche technologique
#### Agents
Il existe deux types de déclencheurs pour les agents.
- Le déclencheur **temporel ou événementiel**: Se déclenche à une cadence ou un instant déterminé. Orchestration type Zena.
- Le **déclencheur par API ou WebHook** : Un client remplit un formulaire sur un site web, ce qui envoie immédiatement une requête à l'agent pour traiter la demande. Développement d'un MCP au niveau de Zena?

> [!NOTE]
> On retrouve parfois le terme déclencheur par "Polling" (Surveillance active), il s'agit en fait d'un agent qui va par exemple surveiller une boîte mail toutes les 30 secondes.
> Le Polling est une forme de déclencheur temporel à haute fréquence.
#### LLM
Analyser les avantages de passer par des LLM grand public.

- **Le risque :** Le "Shadow AI". Si les employés utilisent ChatGPT (version gratuite) pour analyser un code Cobol, le code source devient la propriété d'OpenAI pour leur entraînement.
- **Proposition de solution :** Interdire les versions "Grand Public" gratuites. Exiger des **versions Enterprise** (avec garantie de non-entraînement sur les données) pour tout ce qui dépasse les données publiques.

#### SLM Local
Un SLM est un mini LLM qui va être fine-tuné ou alimenté par un RAG pour répondre à un besoin d'affaire spécifique.
- Réduction des coûts d'utilisation
- Souveraineté des données
- Cas d'usage spécifique et isolé
#### RAG
**Filtrage des Accès (ACL)**: L'agent ne doit pouvoir "chercher" que dans les documents auxquels l'utilisateur qui l'interroge a déjà accès dans la vraie vie.
### La couche des données
Il faut identifier la donnée qui va être consommée par les LLM. Les types de données, à haut niveau sont:
- **Les données externes publique** que l'on retrouve en libre accès sur Internet comme sur lel site ccq.org, la loi R20, wikipedia etc...
- Les données *interne public* comme l'intranet, le Sharepoint commun, flare, les règles d'affaire.
- Les données *interne privées* comme les documents stratégiques, les documents technique sensibles
- Le **code source** du Mainframe, du .Net ou du cobol.
- Les données d'**infrastructure**, concerne les logs d'erreur mais aussi la définition de l'infrastructure.
- Données analytiques - **BI** -
- **Données Synthétiques**: Données générées artificiellement pour le *test* et le développement, ne contenant aucune information réelle mais reproduisant les caractéristiques du domaine métier.
- Les données client ou employé. Ces données sont sensible et on un degré supplémentaire de sensibilité qui est défini par la gouvernance de données. Prie en compte de la gestion des **PII (Personal Identifiable Information)**
### Couche de complexité des projets
- **Niveau 1**: *C'est l'IA "assistante"* (productivité individuelle). Ce sont les projets IA qui ne concernent que les utilisateurs, ou des besoins qui n'ont pas besoin d'être automatisée, comme l'extraction des règles d'affaire ou l'analyse d'un code source.
- **Niveau 2**: *C'est l'IA "opérationnelle" (automatisation simple).* Ce sont des projets IA en production qui vont solliciter un agent. Par exemple l'extraction d'une boîte mail qui classera ensuite les interventions.
- **Niveau  3**:  *C'est l'IA "systémique" (orchestration d'agents).* Ce sont les projets ou l'IA sera utilisée pour remplacer des processus interne, ou plusieurs agents vont parler entre eux pour répondre à un processus métier.
- **Niveau 4**: *IA faisant face au client final*. Le niveau de risque réputationnel et de sécurité (prompt injection) y est maximal.