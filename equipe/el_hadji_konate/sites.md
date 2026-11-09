# 📋 Atelier 1 — `jobs-insights` : Liste des Sites, Analyse de Scraping & Brainstorming V3

**Auteur** : El Hadji Konaté  
**Projet de base** : *JobTailor Canada V2* ➔ **Évolution en projet de classe `jobs-insights` (V3)**  
**Cibles prioritaires** : Stages (TI / Développement logiciel / Réseaux), Jobs étudiants (temps partiel) et Emplois juniors au Québec et au Canada.

---

## 1. Liste des Sites d'Offres d'Emploi et de Stages Analysés

| # | Plateforme | URL de base | Type d'offres ciblées | Format technique détecté | Statut Scraping / POC |
| :-: | :--- | :--- | :--- | :--- | :--- |
| **1** | **Guichet-Emplois Canada** *(Gouvernement du Canada)* | `https://www.guichetemplois.gc.ca` | Stages étudiants, TI, jobs étudiants, tous secteurs au Canada | **SSR HTML + Balises RDFa Schema.org + API Solr JSON + Flux Atom XML** | ✅ **En production (V2) + POC** (`HTTP 200`) |
| **2** | **Jobillico** *(Leader emploi au Québec)* | `https://www.jobillico.com/fr` | Stages TI, emplois PME/grandes entreprises au Québec, jobs étudiants | **SSR HTML + JSON-LD (`@type: JobPosting`)** | ✅ **En production (V2) + POC** (`HTTP 200`) |
| **3** | **Talent.com Canada** *(Agrégateur canadien basé à Montréal)* | `https://ca.talent.com/fr` | Stages en informatique, emplois TI, temps partiel à Québec et Montréal | **SSR HTML + JSON-LD (`ItemList` + `JobPosting` dans `@graph`)** | ✅ **Nouveau POC V3 fonctionnel** (`HTTP 200`) |
| **4** | **Jobboom** *(Portail historique du Québec)* | `https://www.jobboom.com/fr` | Technologies & médias numériques (`_i10lfr`), génie, soutien administratif | **SSR HTML (`div.job_item`) + JSON-LD (`@type: JobPosting`)** | ✅ **Nouveau POC V3 fonctionnel** (`HTTP 200`) |
| **5** | **Remotive** *(Télétravail TI accessible au Canada)* | `https://remotive.com` | Développement logiciel, DevOps, Data, QA (100 % à distance) | **API REST JSON publique** (`/api/remote-jobs`) | ✅ **En production (V2) + POC** (`HTTP 200`) |
| **6** | **Arbeitnow / WorkingNomads** *(APIs ouvertes Tech)* | `https://www.arbeitnow.com/api/job-board-api` | Postes juniors, stages et télétravail en programmation | **API REST JSON publique sans clé** | ✅ **Testable immédiatement** (`HTTP 200`) |
| **7** | **Québec Emploi** *(Services Québec - Gouv. du Québec)* | `https://quebecemploi.gouv.qc.ca` | Emplois régionaux (Capitale-Nationale, Montréal), stages gouvernementaux | **SPA / API interne protégée** | ⚠️ Nécessite Playwright ou entente API |
| **8** | **Indeed Canada / LinkedIn Jobs** | `https://fr.indeed.com` | Toutes catégories | **Protection Cloudflare Turnstile / DataDome (`HTTP 403`)** | ❌ Écarté en HTTP direct (anti-bot strict) |

---

## 2. Analyse Technique Détaillée par Site (Catégories, Filtres, Sélecteurs)

### 2.1. Guichet-Emplois Canada (`guichetemplois.gc.ca`)
* **Pourquoi ce site ?** Source gouvernementale officielle, zéro publicité, données ultra-propres et vérifiées, couvre aussi bien les stages en informatique que les jobs étudiants.
* **URL de recherche et filtres GET identifiés** :
  * Recherche par mot-clé et ville : `https://www.guichetemplois.gc.ca/jobsearch/rechercheemplois?searchstring={mot_cle}&mid={id_ville}&sort=M`
  * **Filtres clés** :
    * `searchstring` : mot-clé (ex. `stage informatique`, `développeur python`, `commis`).
    * `mid` : identifiant numérique officiel de la ville (ex. `22447` = Ville de Québec, `22446` = Montréal, `23587` = Lévis, `22448` = Sherbrooke).
    * `fprov` : code province (ex. `fprov=QC`, `fprov=ON`).
    * `fper` : durée de travail (`F` = Temps plein, `P` = Temps partiel / Job étudiant).
    * `fsrc=16` : filtre spécifique pour les **emplois étudiants / stages**.
* **API interne découverte pour résoudre les villes** :
  * `https://www.guichetemplois.gc.ca/core/ta-cityprovsuggest_fr/select?q={ville}&wt=json` (retourne le `city_id` exact en JSON).
* **Éléments à scraper** :
  1. **Liste des résultats** : liens `<a href="/jobsearch/pers/offredemploi/{id}">` ou `<article id="article-{id}">`.
  2. **Fiche détaillée de l'offre** : balises sémantiques **RDFa Schema.org** directement dans le HTML :
     * `property="title"` (Titre du poste)
     * `property="hiringOrganization"` ➔ `property="name"` (Entreprise)
     * `property="addressLocality"` et `property="addressRegion"` (Ville et Province)
     * `property="baseSalary"` (Salaire horaire ou annuel)
     * `property="description"` (Missions et exigences)
     * `property="validThrough"` (Date limite de candidature)

---

### 2.2. Jobillico Québec (`jobillico.com`)
* **Pourquoi ce site ?** Le plus grand portail privé québécois, très riche en PME de la région de Québec (Capitale-Nationale), Chaudière-Appalaches et Montréal.
* **URL de recherche et filtres GET identifiés** :
  * `https://www.jobillico.com/voir-offres-emploi?skwd={mot_cle}&scty={ville_province}&sort=date`
  * **Filtres clés** :
    * `skwd` : mot-clé recherché (ex. `stage developpeur`, `technicien informatique`).
    * `scty` : ville ciblée (ex. `Québec, QC`, `Montréal, QC`, `Lévis, QC`).
    * `sort=date` : tri par offres les plus récentes.
* **Éléments à scraper** :
  1. **Liste des résultats** : extraction par expression régulière ou BeautifulSoup des liens `/fr/offre-d-emploi/{entreprise}/{slug}/{id}`.
  2. **Fiche détaillée de l'offre** : contient un bloc `<script type="application/ld+json">` au standard **Schema.org `JobPosting`** :
     * `title`, `datePosted`, `validThrough`, `employmentType` (`FULL_TIME`, `PART_TIME`, `INTERN`).
     * `hiringOrganization.name` (Nom de l'employeur).
     * `jobLocation.address.addressLocality` et `addressRegion`.
     * `description` (HTML contenant des puces `<li>` faciles à extraire pour isoler les missions et compétences).

---

### 2.3. Talent.com Canada (`ca.talent.com/fr`) — *Nouveau pour V3*
* **Pourquoi ce site ?** Entreprise québécoise (Montréal) qui agrège énormément d'offres de stages collégiaux/universitaires et d'emplois TI au Québec, avec un balisage JSON-LD complet dès la page de recherche.
* **URL de recherche et filtres GET identifiés** :
  * `https://ca.talent.com/fr/jobs?k={mot_cle}&l={ville}&date={periode}`
  * **Filtres clés** :
    * `k` : mot-clé (ex. `stage informatique`, `developpeur python`).
    * `l` : localisation (ex. `Québec, QC`, `Montréal, QC`).
    * `workplace` : mode de travail (`remote`, `hybrid`).
* **Éléments à scraper (Double JSON-LD)** :
  1. **Sur la page de recherche** : un bloc `<script type="application/ld+json">` contient un `@graph` avec un objet `"@type": "ItemList"` (`#jobList`) qui donne directement les 20 URLs propres des offres (`https://ca.talent.com/fr/view?id={id}`) sans avoir à parser le DOM HTML !
  2. **Sur la page d'une offre (`/fr/view?id=...`)** : le `@graph` JSON-LD contient l'objet `"@type": "JobPosting"` complet avec :
     * `title`, `description`, `datePosted`, `employmentType`, `industry`, `occupationalCategory`.
     * `hiringOrganization` (`name`, `logo`).
     * `jobLocation.address` (`addressLocality`, `addressRegion`) + coordonnées GPS exactes (`jobLocation.geo.latitude` / `longitude` — idéal pour une carte interactive en V3 !).

---

### 2.4. Jobboom Québec (`jobboom.com/fr`) — *Nouveau pour V3*
* **Pourquoi ce site ?** Classé par secteurs métiers très précis au Québec, avec un sous-secteur dédié aux technologies et médias numériques.
* **URL de navigation et filtres par segments d'URL** :
  * **Par secteur d'activité (`_i{ID}lfr`)** :
    * `_i10lfr` : **Technologies et médias numériques** (`/fr/technologies-medias-numeriques/_i10lfr?sortBy=date&page=1`)
    * `_i11lfr` : Génie, sciences et techniques scientifiques
    * `_i8lfr` : Vente, achat et service à la clientèle
    * `_i7lfr` : Soutien administratif
    * `_i45lfr` : Construction, production et manutention
  * **Par profession TI précise (`_i10m{ID}lfr`)** :
    * `_i10m751lfr` : Développeur / Intégrateur Web
    * `_i10m695lfr` : Analyste de données (Data Analyst)
    * `_i10m90lfr` : Analyste en assurance qualité (QA)
    * `_i10m92lfr` : Analyste en sécurité TI (Cybersécurité)
    * `_i10m89lfr` : Administrateur de bases de données (DBA)
    * `_i10m97lfr` : Administrateur de réseaux
    * `_i10m100lfr` : Administrateur de systèmes
* **Éléments à scraper** :
  1. **Liste des résultats** : chaque carte d'offre est un `<div class="job_item" data-url="/fr/offre-emploi/{slug}_p{id}">` contenant aussi `<span class="jobCityProv">` et `<span class="jobSector">`. *(Astuce : ignorer les 3 premières cartes qui ont la classe `job_item_sponsorship` car ce sont des annonces commanditées).*
  2. **Fiche détaillée de l'offre** : contient un `<script type="application/ld+json">` de type `"@type": "JobPosting"` avec `title`, `description`, `datePosted`, `validThrough`, `hiringOrganization`, `jobLocation` et `baseSalary`.

---

### 2.5. Remotive (`remotive.com/api/remote-jobs`)
* **Pourquoi ce site ?** API REST JSON officielle et gratuite pour les postes Tech 100 % en télétravail.
* **Filtres GET** :
  * `category=software-dev` (ou `data`, `devops`, `qa`, `cybersecurity`)
  * `search={terme_anglais}` (ex. `python`, `backend`, `react`)
  * `limit=20`
* **Filtrage côté Python** : vérifier le champ `candidate_required_location` pour ne retenir que `"Canada"`, `"North America"`, `"Americas"` ou `"Worldwide"`.

---

## 3. Brainstorming V3 : De *JobTailor V2* vers le Projet de Classe `jobs-insights`

Puisque le projet devient collaboratif pour toute la classe sous le nom **`jobs-insights`**, voici les axes d'évolution pour passer de la **V2 (Adaptation CV + Collecte en direct)** à une **V3 (Plateforme d'Intelligence du Marché de l'Emploi & Carrière)** :

### Axe 1 — Architecture Modulaire « 1 Étudiant = 1 Connecteur (Scraper) »
* Dans la V2, nous avons déjà créé l'interface abstraite `BaseJobWatcher` (`src/watchers/base_watcher.py`) et le `WatcherRouter` (`src/watchers/router.py`).
* **Pour la classe** : chaque étudiant (ou équipe) peut développer son propre adaptateur dans `src/watchers/sources/` (ex. `talent_ca_watcher.py`, `jobboom_watcher.py`, `grenier_watcher.py`, `workopolis_watcher.py`) qui retourne un objet standardisé `JobPosting`. Le routeur les agrège et les dédoublonne automatiquement par hash SHA-256 !

### Axe 2 — Module `Insights` : Analyse Statistique du Marché Québécois (Nouveau en V3)
* Au lieu de seulement traiter une candidature à la fois, accumuler les offres scrapées dans une base de données partagée (SQLite / PostgreSQL) pour générer un **Tableau de Bord analytique (`jobs-insights`)** :
  1. **Top des technologies les plus demandées à Québec vs Montréal** (ex. % d'offres demandant Python, C#, React, Docker, SQL, AWS).
  2. **Baromètre des salaires par métier et séniorité** (extraction automatique de `baseSalary` depuis les JSON-LD / RDFa).
  3. **Carte interactive des offres et stages** (grâce aux coordonnées GPS `latitude`/`longitude` extraites du JSON-LD de Talent.com et des villes de Guichet-Emplois).
  4. **Indice de rareté des stages** : suivi quotidien du nombre de nouveaux stages TI publiés par semaine à l'approche des sessions d'hiver et d'été.

### Axe 3 — Système d'Alertes & Suivi de Candidatures (Kanban Étudiant)
* Détection quotidienne des **nouvelles offres** (comparaison des `content_hash` entre hier et aujourd'hui).
* Tableau de suivi personnel pour chaque étudiant : *À postuler* ➔ *CV adapté généré* ➔ *Candidature envoyée* ➔ *Entrevue*.
