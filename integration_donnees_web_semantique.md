1) Qu’est-ce que l’hétérogénéité des données dans l’intégration ?

L’hétérogénéité est l'ensemble des différences et incompatibilités entre sources de données qui empêchent leur fusion directe.
Elle se manifeste à plusieurs niveaux (format, vocabulaire, granularité, représentation, qualité, etc.) et complique requêtes,
analyses et raisonnement inter-sources.

2) Types d’hétérogénéités (structurale, syntaxique, sémantique)

.Hétérogénéité syntaxique
— différences de format ou d’encodage (CSV vs JSON vs XML, encodage UTF-8 vs ISO-8859-1).
Solution : normalisation/parseurs, nettoyage d’encodage.

.Hétérogénéité structurale (ou schématique)
— mêmes informations stockées selon des modèles différents (colonne date_naissance vs dob ; relation imbriquée vs table séparée).
Solution : schéma cible, mappings, ETL, transformation, ontologies pour exposer une structure commune.

.Hétérogénéité sémantique
— divergence de sens/vocabulaire : synonymes (client vs customer), homonymes, différences de granularité (ville vs métropole), unités (m, ft).
Solution : ontologies, dictionnaires, alignement sémantique, normalisation d’unités.

.Autres : hétérogénéité de qualité (valeurs manquantes / bruit), temporelle (différences dans le temps), et de provenance (confiance/source).

3) Pourquoi en tenir compte pour une intégration Web sémantique ?

.Pour préserver le sens lors de l’agrégation (éviter mauvaises correspondances).

.Pour rendre les données interopérables entre applications et équipes.

.Pour permettre un raisonnement automatique (les moteurs logiques ont besoin d’un sens partagé).

.Pour assurer qualité et traçabilité (provenance, confiance).
.Ignorer l’hétérogénéité conduit à des erreurs d’analyse, doublons, ou décisions erronées.

4) Comment RDF / ontologies aident à réduire incohérences et harmoniser

.RDF (triplets sujet-predicat-objet) : modèle flexible qui représente n’importe quelle relation, facilitant l’uniformisation des structures hétérogènes en graphes alignés.

.Ontologies (OWL, SKOS) : définissent concepts, relations, contraintes et axiomes (subClassOf, domain/range, équivalence), permettant :

    .normalisation des vocabulaires (synonymes, équivalences),

    .validation (contrainte de cardinalité),

    .enrichissement par inférence (raisonneurs OWL).

.SPARQL : requêtes fédérées sur sources RDF, indépendantes du schéma physique.

.Provenance (PROV) et métadonnées : aident à conserver origine et confiance.
→ En pratique on construit un modèle de référence (ontology / vocabulary), on mappe/transforme les sources vers RDF et on applique alignement et raisonnement.

5) Rôle d’un Knowledge Graph (KG) dans gestion et représentation des données intégrées

.Modèle unificateur : rassemble entités et relations provenant de sources différentes dans un graphe connecté.

.Désambiguïsation / fusion d’entités (entity resolution) : relie mentions hétérogènes d’un même objet.

.Support du raisonnement : inférences structurelles et logiques (via ontologies, règles).

.Richesse contextuelle : stocke attributs, provenance, temporalité, confiance.

.Interface pour applications : QA, recherche sémantique, analyses graphiques, pipelines ML.
→ Le KG devient la « couche sémantique » entre données brutes et usages.

6) Pourquoi le format graphe facilite exploration & raisonnement vs bases relationnelles classiques

.Flexibilité de schéma : ajout simple de nouveaux types de nœuds/relations sans remodeler tout le schéma relationnel.

.Requêtes relationnelles profondes : parcours multi-sauts (chemins, voisinage) natifs et efficaces.

.Représentation naturelle des réseaux (relations multiples, hypergraphes approximés) : mieux adapté à réseaux sociaux, chaînes d’approvisionnement, ontologies.

.Raisonnement logique : intégration directe d’axiomes OWL/RDFS et règles, permettant inférences sur topologie.

.Interopérabilité & Linked Data : URI globales pour entités, facilitant liens externes (sameAs, owl:sameAs).
.En revanche, les SGBD relationnels restent meilleurs pour transactions massives et agrégations numériques; souvent on combine les deux.

7) Contribution des KGs aux applications d’apprentissage automatique

.Enrichissement de features : propriétés et voisinages comme features (embeddings de KG).

.Meilleure contextualisation : relations explicites permettent désambiguïsation (ex : « Apple » fruit vs entreprise).

.Transfert de connaissances : KG fournit connaissances structurées non triviales que modèles statistiques exploitent.

.Amélioration du rappel/pertinence dans recherche, recommandation et QA.

.Support pour modèles symboliques/neuromorphes : hybridation symbolique–neuronale (ex : règles + réseaux).
  Exemples : KG embeddings (TransE, ComplEx), GNNs sur KG pour prédiction de liens, et features pour classifieurs.

8) Manières d’utiliser les LLM pour enrichir / interagir avec un KG

.Extraction d’information : NER/relation extraction depuis texte -> nouvelles entités/relations ajoutées au KG.

.Normalisation et mapping : LLM pour normaliser labels, mapper termes non standard aux classes d’une ontologie.

.Complétion de KG : génération de triples candidats (zone hypothétique) puis validation.

.Question-Answering (QA) : LLM interroge le KG (via SPARQL) ou combine KG + contexte pour répondre.

.Reformulation de requêtes : transformer une question utilisateur en requête SPARQL.

.Explication & Naturalisation : produire explications humaines pour relations extraites.

.Embeddings hybrides : combiner embeddings textuels LLM et embeddings structurels KG pour alignement entité-texte.

9) Défis spécifiques à l’usage conjoint KG + LLM

.Biais / hallucinations du LLM : génération de faits non vérifiés qui, si ingérés, polluent le KG.

.Contrôle de qualité & vérifiabilité : comment valider automatiquement des triples produits par LLM ? (provenance + score de confiance nécessaires).

.Alignement vocabulaire/ontologie : LLM peut proposer termes non alignés aux vocabulaires existants.

.Scalabilité : maintenir KG massifs et requêtes temps réel tout en appelant LLM coûteux.

.Sécurité / confidentialité : fuite d’information si LLM est utilisé sur données sensibles.

.Interprétabilité : décisions du LLM peu transparentes vs axiomes explicites du KG.

.Boucle de rétroaction : risque d’amplification d’erreurs si LLM s’entraîne sur données enrichies qu’il a lui-même produites.

10) Comment les LLM peuvent aider à identifier, corriger, compléter des données manquantes/ambiguës

.Suggestion d’entités/valeurs manquantes : en fournissant contexte, LLM propose valeurs plausibles (avec score).

.Désambiguïsation contextuelle : LLM peut décider quel sens d’un mot est pertinent selon contexte local et proposer lien vers entité KG.

.Fuzzy matching & canonicalisation : rapprocher variantes textuelles (abbrev., fautes) vers une forme canonique.

.Génération d’attributs dérivés : calculer/extraire des descriptions, résumés, catégories.

Priorisation humaine : LLM peut produire explications/rationnel pour que des annotateurs valident rapidement.
Bonne pratique : ne pas ingérer automatiquement ; utiliser validation statistique, règles logiques, ou approbation humaine.

11) Collaboration entre modèles sémantiques (KG) et modèles statistiques (LLM) : perspectives

.Hybridation symbolique-neuronale : combiner robustesse et explicabilité des KG avec généralisation des LLM.

.RAG (Retrieval-Augmented Generation) : récupérer passages/KG subgraphes pertinents pour conditionner la génération LLM.

.Boucles d’auto-amélioration : KG alimente LLM (contexte), LLM enrichit KG (extr.) — avec contrôles.

.IA plus fiable et explicable : requêtes raisonnées via KG + narration par LLM pour auditabilité.

.Nouveaux services : assistants sémantiques, agents autonomes qui planifient en combinant règles et génération.
Ces collaborations ouvrent des applications plus intelligentes, contextualisées et traçables.

12) Scénarios métiers prometteurs:

.Santé : intégration dossiers patients, recherche biomédicale (KG pour relations gènes-maladies, LLM pour extraction d’articles).

.Finance : KG des entités économiques + LLM pour analyses de news, détection fraude, KYC.

.Supply chain / logistique : traçabilité et raisonnement sur réseaux complexes, résolution d’incidents.

.E-commerce / recommandation : KG produit/catégorie + LLM pour descriptions, recherche conversationnelle.

.Assistance juridique / conformité : KG de régulations + LLM pour interprétation et réponses naturelles.

.Recherche & R&D : découverte de connaissances (link prediction), hypothèse generation.

.Support client intelligent : KG des produits + LLM pour dialogues multimodaux et résolutions guidées
