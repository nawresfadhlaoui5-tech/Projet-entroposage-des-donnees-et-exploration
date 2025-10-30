### 1) Définition de l'hétérogénéité des données dans l’intégration

L’hétérogénéité est l'ensemble des différences et incompatibilités entre sources de données qui empêchent leur fusion directe.  
Elle se manifeste à plusieurs niveaux (format, vocabulaire, granularité, représentation, qualité, etc.) et complique les requêtes, analyses et raisonnements inter-sources.

### 2) Types d’hétérogénéités (structurale, syntaxique, sémantique)

**Hétérogénéité syntaxique**  
- Différences de format ou d’encodage (CSV vs JSON vs XML, UTF-8 vs ISO-8859-1)  
- **Solution :** normalisation/parseurs, nettoyage d’encodage

**Hétérogénéité structurale (ou schématique)**  
- Mêmes informations stockées selon des modèles différents (colonne `date_naissance` vs `dob`; relation imbriquée vs table séparée)  
- **Solution :** schéma cible, mappings, ETL, transformation, ontologies pour exposer une structure commune

**Hétérogénéité sémantique**  
- Divergence de sens/vocabulaire : synonymes (`client` vs `customer`), homonymes, différences de granularité (ville vs métropole), unités (m vs ft)  
- **Solution :** ontologies, dictionnaires, alignement sémantique, normalisation d’unités

**Autres types**  
- Qualité : valeurs manquantes / bruit  
- Temporelle : différences dans le temps  
- Provenance : confiance/source

### 3) Importance de la prise en compte de l’hétérogénéité pour le Web sémantique

- **Préserver le sens lors de l’agrégation (éviter mauvaises correspondances)**  
  Quand on combine plusieurs sources de données, il est possible que des informations similaires aient des significations légèrement différentes ou des formats différents.  

  **Exemple :** une colonne `ville` dans une source peut contenir « Paris », tandis qu’une autre source utilise « Paris, France ». Sans harmonisation, un système pourrait traiter ces deux valeurs comme distinctes, ce qui fausse les résultats.  

  **Importance :** préserver le sens permet que les données agrégées restent cohérentes et qu’aucune erreur de correspondance ne se produise.

- **Rendre les données interopérables entre applications et équipes**  
  Les données viennent souvent de systèmes différents (bases relationnelles, fichiers CSV, API…). Chaque application ou équipe peut avoir ses propres conventions et formats.  

  **Exemple :** un département utilise `dob` pour « date de naissance » et un autre `date_naissance`.  

  **Importance :** standardiser et harmoniser les données permet à tous les systèmes et équipes de les utiliser correctement, sans confusion ou transformation manuelle à chaque fois.

- **Permettre un raisonnement automatique (les moteurs logiques ont besoin d’un sens partagé)**  
  Les moteurs logiques ou systèmes intelligents analysent les données pour tirer des conclusions ou détecter des patterns. Pour fonctionner correctement, ils ont besoin que les concepts et relations soient clairement définis et cohérents.  

  **Exemple :** si un moteur doit détecter les clients actifs, il doit comprendre que `customer` dans une source et `client` dans une autre désignent le même concept.  

  **Importance :** sans un sens partagé, les algorithmes risquent de produire des résultats incorrects ou incomplets.

- **Assurer la qualité et la traçabilité (provenance, confiance)**  
  Chaque donnée doit être fiable et son origine connue. La provenance indique d’où vient l’information et permet de vérifier sa fiabilité.  

  **Exemple :** une donnée de transaction venant d’une base interne peut être plus fiable qu’une donnée extraite d’une source externe non vérifiée.  

  **Importance :** cela permet de détecter les erreurs, de vérifier les décisions prises à partir des données et d’avoir confiance dans les analyses.


> Ignorer l’hétérogénéité conduit à des erreurs d’analyse, doublons ou décisions erronées.

### 4) Comment RDF / ontologies aident à réduire les incohérences

**RDF (triplets sujet-prédicat-objet)**  
- Modèle flexible représentant n’importe quelle relation, facilitant l’uniformisation des structures hétérogènes en graphes alignés  

**Ontologies (OWL, SKOS)**  
- Définissent concepts, relations, contraintes et axiomes (`subClassOf`, `domain/range`, équivalence)  
- **Avantages :**  
  - Normalisation des vocabulaires (synonymes, équivalences)  
  - Validation (contraintes de cardinalité)  
  - Enrichissement par inférence (raisonneurs OWL)  

**SPARQL**  
- Requêtes fédérées sur des sources RDF, indépendantes du schéma physique. 

**Provenance (PROV) et métadonnées**  
- aident à conserver l'origine et la confiance.
> Pratique : construire un modèle de référence (ontology/vocabulary), mapper/transformer les sources vers RDF et appliquer alignement + raisonnement.

### 5) Rôle d’un Knowledge Graph (KG)
Un Knowledge Graph (KG) est un graphe représentant des entités et leurs relations, où nœuds et liens peuvent contenir des propriétés ou métadonnées.Ces principales roles sont:
- **Modèle unificateur :** rassemble les entités et les relations de sources différentes dans un graphe connecté  
- **Désambiguïsation / fusion d’entités :** identifie et unifie les mentions d’un même objet  
- **Support du raisonnement :** inférences structurelles et logiques via ontologies et règles  
- **Richesse contextuelle :** attributs, provenance, temporalité, confiance  
- **Interface pour applications :** recherche, QA, analyse graphique, pipelines Machine Learning  

> Le KG agit comme une couche sémantique entre données brutes et usages, facilitant intégration, cohérence et exploitation intelligente.

### 6) Pourquoi le format graphe facilite exploration & raisonnement vs bases relationnelles

- **Flexibilité de schéma :** ajout simple de nouveaux types de nœuds/relations  
- **Requêtes relationnelles profondes :** parcours multi-sauts natifs et efficaces  
- **Représentation naturelle des réseaux :** mieux adapté à réseaux sociaux, chaînes d’approvisionnement, ontologies  
- **Raisonnement logique :** axiomes OWL/RDFS et règles intégrés pour inférences  
- **Interopérabilité & Linked Data :** URI globales pour entités, liens externes (`sameAs`, `owl:sameAs`)  

> Remarque : SGBD relationnels restent meilleurs pour transactions massives et agrégations numériques.

### 7) Contribution des Knowledge Graphs (KGs) aux applications d’apprentissage automatique

- **Enrichissement de features :** utilisation des propriétés et voisinages des entités comme features (embeddings de KG)  
- **Meilleure contextualisation :** relations explicites permettant la désambiguïsation (ex : « Apple » fruit vs entreprise)  
- **Transfert de connaissances :** le KG fournit des connaissances structurées exploitables par les modèles statistiques  
- **Amélioration du rappel et de la pertinence :** dans les tâches de recherche, recommandation et question-answering (QA)  
- **Support pour modèles symboliques/neuromorphes :** hybridation de règles et réseaux neuronaux pour enrichir les modèles  

> **Exemples :** KG embeddings (TransE, ComplEx), Graph Neural Networks (GNNs) sur KG, et features dérivées pour classifieurs

### 8) Manières d’utiliser les LLM pour enrichir / interagir avec un KG

- **Extraction d’information :** NER (Named Entity Recognition) et extraction de relations depuis le texte → nouvelles entités et relations ajoutées au KG  
- **Normalisation et mapping :** le LLM est utilisé pour normaliser les labels et mapper les termes vers les classes d’une ontologie  
- **Complétion de KG :** génération de triples candidats suivie d’une validation avant ingestion  
- **Question-Answering (QA) :** le LLM interroge le KG via SPARQL ou combine le KG avec un contexte externe pour répondre  
- **Reformulation de requêtes :** transformation des questions des utilisateurs en requêtes SPARQL valides  
- **Explication & naturalisation :** production d’explications compréhensibles par l’homme pour les relations extraites  
- **Embeddings hybrides :** combinaison des embeddings textuels du LLM avec les embeddings structurels du KG pour alignement entité-texte


### 9) Défis de l’usage conjoint KG + LLM

- **Biais et hallucinations du LLM :** génération de faits non vérifiés pouvant polluer le KG  
- **Contrôle de qualité et vérifiabilité :** nécessité de valider automatiquement les triples produits  
- **Alignement vocabulaire / ontologie :** risque que les termes proposés par le LLM ne correspondent pas aux vocabulaires existants  
- **Scalabilité :** maintenir de gros KG et répondre à des requêtes en temps réel tout en utilisant un LLM coûteux  
- **Sécurité et confidentialité :** protection des données sensibles lors de l’interaction LLM-KG  
- **Interprétabilité :** décisions du LLM parfois opaques par rapport aux axiomes explicites du KG  
- **Boucle de rétroaction :** risque d’amplification d’erreurs si le LLM s’entraîne sur les données qu’il a lui-même enrichies


### 10) Comment les LLM aident à identifier, corriger et compléter des données manquantes ou ambiguës

- **Suggestion d’entités ou valeurs manquantes :** le LLM propose des valeurs plausibles en tenant compte du contexte et d’un score de confiance  
- **Désambiguïsation contextuelle :** permet de lier correctement les mentions aux entités du KG  
- **Fuzzy matching et canonicalisation :** rapproche les variantes textuelles (abréviations, fautes, synonymes) vers une forme canonique  
- **Génération d’attributs dérivés :** création de descriptions, résumés ou catégories supplémentaires à partir des données existantes  
- **Priorisation humaine :** le LLM fournit des explications et rationnels pour permettre une validation rapide par des annotateurs  

> **Bonne pratique :** ne pas ingérer automatiquement ; utiliser validation statistique, règles logiques ou approbation humaine avant intégration dans le KG.


### 11) Collaboration entre modèles sémantiques (KG) et modèles statistiques (LLM)

- **Hybridation symbolique-neuronale :** combine la robustesse et l’explicabilité du KG avec la capacité de généralisation des LLM  
- **RAG (Retrieval-Augmented Generation) :** récupération de passages ou subgraphes pertinents pour alimenter le LLM  
- **Boucles d’auto-amélioration :** le KG alimente le LLM, et le LLM enrichit le KG via extraction, avec contrôles  
- **IA plus fiable et explicable :** requêtes raisonnées via KG combinées à une narration générée par le LLM  
- **Nouveaux services :** assistants sémantiques et agents autonomes qui combinent règles explicites et génération automatique


### 12) Scénarios métiers prometteurs

- **Santé :** intégration des dossiers patients et recherche biomédicale (KG pour relations gènes-maladies, LLM pour extraction d’articles)  
- **Finance :** KG des entités économiques combiné à LLM pour analyses de news, détection de fraude, KYC  
- **Supply chain / logistique :** traçabilité et raisonnement sur des réseaux complexes  
- **E-commerce / recommandation :** KG produit/catégorie + LLM pour descriptions et recherche conversationnelle  
- **Assistance juridique / conformité :** KG de régulations + LLM pour interprétation et réponses naturelles  
- **Recherche & R&D :** découverte de connaissances (link prediction) et génération d’hypothèses  
- **Support client intelligent :** KG des produits + LLM pour dialogues multimodaux et résolutions guidées

