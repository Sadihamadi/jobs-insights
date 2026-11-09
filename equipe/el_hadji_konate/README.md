# Dossier Personnel — El Hadji Konaté (`jobs-insights` - Atelier 1)

Ce dossier contient mes livrables pour l'**Atelier 1** du projet de classe **`jobs-insights` (V3)**, basé sur l'évolution de mon projet **JobTailor Canada V2** :

1. **[`sites.md`](sites.md)** :
   - Liste détaillée des sites d'offres d'emploi et de stages au Québec et au Canada (*Guichet-Emplois Canada, Jobillico, Talent.com Canada, Jobboom Québec, Remotive, Arbeitnow, Québec Emploi, Indeed/LinkedIn*).
   - Identification technique complète de ce qui est à scraper : filtres d'URL (`GET` / segments), catégories TI (`_i10lfr`), filtres stages (`fsrc=16`), balises sémantiques **RDFa Schema.org**, blocs **JSON-LD (`ItemList` et `JobPosting`)**, coordonnées GPS et APIs REST JSON.
   - Pistes de **Brainstorming V3** pour le projet de classe `jobs-insights`.

2. **[`poc_scrapers_atelier1.py`](poc_scrapers_atelier1.py)** :
   - Script Python autonome (zéro dépendance externe requise hors bibliothèque standard Python) démontrant le scraping en direct (`HTTP 200`) sur :
     - **Talent.com Canada** (`ca.talent.com/fr`) : extraction double JSON-LD (`ItemList` ➔ `JobPosting` + coordonnées GPS).
     - **Jobboom Québec** (`jobboom.com/fr`) : navigation dans le secteur Technologies (`_i10lfr`) + extraction `JobPosting`.
     - **Guichet-Emplois Canada** (`guichetemplois.gc.ca`) : résolution de ville via l'API Solr gouvernementale + extraction des fiches RDFa.
     - **Jobillico Québec** (`jobillico.com`) : extraction des liens d'offres et lecture du bloc JSON-LD `JobPosting`.

## Exécution rapide du POC

```powershell
python poc_scrapers_atelier1.py
```
