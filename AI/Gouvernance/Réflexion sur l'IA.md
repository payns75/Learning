### Historique

### La couche technologique

### La couche des données

Il faut identifier la donnée qui va être consommée par les LLM. Les types de données, à haut niveau sont:
- Les données public que l'on retrouve en libre accès sur Internet comme sur lel site ccq.org, la loi R20, wikipedia etc...
- Les données interne public comme l'intranet, le Sharepoint commun, flare, les règles d'affaire.
- Les données interne privée comme les documents stratégiques, les documents technique sensibles
- Le code source du Mainframe, du .Net ou du cobol.
- Les données d'infrastructure
- Les données client ou employé. Ces données sont sensible et on un degré supplémentaire de sensibilité qui est défini par la gouvernance de données.

### Couche de complexité
- Cas 1: Ce sont les projets IA qui ne concernent que les utilisateurs, ou des besoins qui n'ont pas besoin d'être automatisée, comme l'extraction des règles d'affaire ou l'analyse d'un code source.
- Cas 2: Ce sont des projets IA en production qui vont solliciter un agent. Par exemple l'extraction d'une boîte mail qui classera ensuite les interventions.
- Cas 3: Ce sont les projets ou l'IA sera utilisée pour remplacer des processus interne, ou plusieurs agents vont parler entre eux pour répondre à un processus métier.

### Cas du SLM

Un SLM est un mini LLM qui va être fine tuné ou alimenté par un RAG pour répondre à un besoin d'affaire spécifique.