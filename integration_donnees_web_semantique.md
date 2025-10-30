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

- Préserver le sens lors de l’agrégation (éviter mauvaises correspondances)  
- Rendre les données interopérables entre applications et équipes  
- Permettre un raisonnement automatique (les moteurs logiques ont besoin d’un sens partagé)  
- Assurer qualité et traçabilité (provenance, confiance)  

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
- Requêtes fédérées sur sources RDF, indépendantes du schéma physique  

**Provenance (PROV) et métadonnées**  
- Conservent origine et confiance  

> Pratique : construire un modèle de référence (ontology/vocabulary), mapper/transformer les sources vers RDF et appliquer alignement + raisonnement.

### 5) Rôle d’un Knowledge Graph (KG)

- **Modèle unificateur :** rassemble entités et relations de sources différentes dans un graphe connecté  
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

### 7) Contribution des KGs aux applications d’apprentissage automatique

- Enrichissement de features : propriétés et voisinages comme features (embeddings de KG)  
- Meilleure contextualisation : relations explicites pour désambiguïsation (ex: « Apple » fruit vs entreprise)  
- Transfert de connaissances : KG fournit connaissances structurées exploitables par modèles statistiques  
- Amélioration du rappel/pertinence : recherche, recommandation, QA  
- Support pour modèles symboliques/neuromorphes : hybridation règles + réseaux  

> Exemples : KG embeddings (TransE, ComplEx), GNNs sur KG, features pour classifieurs

### 8) Manières d’utiliser les LLM pour enrichir / interagir avec un KG

- Extraction d’information : NER / relation extraction depuis texte → nouvelles entités/relations ajoutées au KG  
- Normalisation et mapping : LLM pour labels, mapping vers classes d’ontologie  
- Complétion de KG : génération de triples candidats puis validation  
- Question-Answering : LLM interroge KG via SPARQL ou combine KG + contexte  
- Reformulation de requêtes : transformer questions utilisateurs en SPARQL  
- Explication & naturalisation : produire explications humaines pour relations extraites  
- Embeddings hybrides : combiner embeddings textuels LLM et embeddings structurels KG

### 9) Défis de l’usage conjoint KG + LLM

- Biais / hallucinations du LLM → génération de faits non vérifiés  
- Contrôle de qualité & vérifiabilité → validation automatique des triples  
- Alignement vocabulaire/ontologie  
- Scalabilité : maintenir KG massifs et requêtes temps réel avec LLM coûteux  
- Sécurité / confidentialité  
- Interprétabilité : décisions LLM vs axiomes explicites du KG  
- Boucle de rétroaction : risque d’amplification d’erreurs

### 10) Comment les LLM aident à identifier/corriger/compléter des données manquantes/ambiguës

- Suggestion d’entités/valeurs manquantes avec contexte et score  
- Désambiguïsation contextuelle pour lier au KG  
- Fuzzy matching & canonicalisation : rapproche variantes textuelles vers forme canonique  
- Génération d’attributs dérivés : descriptions, résumés, catégories  
- Priorisation humaine : LLM propose rationnel pour validation rapide  

> Bonne pratique : validation statistique, règles logiques ou approbation humaine avant ingestion automatique.

### 11) Collaboration modèles sémantiques (KG) et modèles statistiques (LLM)

- Hybridation symbolique-neuronale : robustesse & explicabilité du KG + généralisation des LLM  
- RAG (Retrieval-Augmented Generation) : récupération passages / subgraphes pertinents pour LLM  
- Boucles d’auto-amélioration : KG alimente LLM, LLM enrichit KG (extr.) avec contrôles  
- IA plus fiable et explicable : requêtes raisonnées via KG + narration par LLM  
- Nouveaux services : assistants sémantiques, agents autonomes combinant règles et génération

### 12) Scénarios métiers prometteurs

- Santé : intégration dossiers patients, recherche biomédicale (KG pour gènes-maladies, LLM pour extraction d’articles)  
- Finance : KG des entités économiques + LLM pour analyses de news, détection fraude, KYC  
- Supply chain / logistique : traçabilité et raisonnement sur réseaux complexes  
- E-commerce / recommandation : KG produit/catégorie + LLM pour descriptions, recherche conversationnelle  
- Assistance juridique / conformité : KG de régulations + LLM pour interprétation et réponses naturelles  
- Recherche & R&D : découverte de connaissances (link prediction), hypothèses  
- Support client intelligent : KG produits + LLM pour dialogues multimodaux et résolutions guidées
