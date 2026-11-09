# Sites d'offres d'emploi et de stages

**Profil ciblé :** emplois et stages en développement logiciel / informatique au Québec, avec intérêt pour les possibilités à distance.


## Sites à surveiller

| Site | Pourquoi il est pertinent | Filtres et renseignements à relever | Approche de collecte |
|---|---|---|---|
| [Guichet-Emplois (Job Bank)](https://www.jobbank.gc.ca/jobsearch/jobsearch?searchstring=developpeur&locationstring=Quebec) | Site officiel fédéral; offres au Québec et emplois étudiants ou stages. | Mot-clé, province, date de publication, catégorie, étudiant/stage, temps plein ou partiel, langue, salaire, durée, lieu de travail (sur place, hybride ou à distance). Une offre contient notamment le titre, le lieu, l'employeur, le salaire et un lien. | **POC prioritaire :** le site fournit un flux Atom/RSS pour les résultats. Son `robots.txt` indique un délai de crawl de 5 secondes. Faire une requête à la fois, sans boucle de pagination; le script de cette recherche reprend les paramètres de catégorie vus dans le flux et filtre aussi les titres et lieux au Québec. |
| [Québec emploi](https://www.quebecemploi.gouv.qc.ca/plateforme-emploi/?lang=fr) | Portail du gouvernement du Québec; il annonce des postes à temps plein, à temps partiel, saisonniers, étudiants et des stages rémunérés. | Région, type ou durée d'emploi, niveau de formation, domaine d'études et type d'offre (régulier, étudiant ou stage). Certaines fonctions demandent un profil candidat. | À consulter manuellement. Avant tout POC, vérifier les conditions du portail et l'existence d'un flux ou d'une API publique. |
| [Emplois en ligne du gouvernement du Québec](https://emplois.carrieres.gouv.qc.ca/plateforme-emploi) et [emplois étudiants et stages](https://www.carrieres.gouv.qc.ca/etudiants-et-stagiaires/) | Source directe pour les postes dans les ministères et organismes, y compris les possibilités étudiantes. | Domaine, région, statut étudiant/stage et renseignements propres à chaque offre. | À consulter manuellement; vérifier les conditions d'utilisation et les possibilités de flux avant l'automatisation. |
| [Jobillico](https://www.jobillico.com/recherche-emploi?sk=developpeur&l=Qu%C3%A9bec) | Site québécois qui peut faire ressortir des employeurs et des postes locaux. | Mot-clé et lieu dans la recherche; les fiches consultées présentent notamment les exigences, le niveau d'études, l'expérience et les langues. | À consulter manuellement pour l'instant. Le `robots.txt` comporte des restrictions sur certaines URL de recherche et de pagination; ne pas automatiser sans vérifier l'URL et les conditions applicables. |
| [LinkedIn Jobs](https://ca.linkedin.com/jobs/search/?keywords=stage%20d%C3%A9veloppement%20logiciel&location=Quebec) | Utile pour repérer les stages affichés par des entreprises technologiques et suivre leurs offres. | Mot-clé, lieu, date, niveau d'expérience, type d'emploi et modalités de travail selon les résultats de recherche. | **Consultation manuelle seulement.** Le `robots.txt` de LinkedIn indique que l'accès automatisé sans autorisation expresse est interdit. |
| [Indeed Canada](https://ca.indeed.com/) | Agrégateur généraliste avec des offres de stages et de postes juniors. | Mots-clés, lieu, date, type d'emploi et autres filtres proposés dans l'interface. | À consulter manuellement. La page de recherche n'était pas accessible pendant cette vérification (HTTP 403); ne pas contourner le blocage ni automatiser sans autorisation claire. |


## Sources et limites de la recherche

- [Filtres de recherche du Guichet-Emplois](https://www.jobbank.gc.ca/jobsearch/jobsearch?searchstring=developpeur&locationstring=Quebec) et [robots.txt](https://www.jobbank.gc.ca/robots.txt).
- [Québec emploi — consulter les offres](https://www.quebec.ca/emploi/trouver-emploi-stage/consulter-offres-emplois).
- [Jobillico — robots.txt](https://www.jobillico.com/robots.txt).
- [LinkedIn — robots.txt](https://www.linkedin.com/robots.txt).
- [Indeed Canada — robots.txt](https://ca.indeed.com/robots.txt).

Les filtres affichés, les offres et les règles des sites peuvent changer. Cette liste sert de point de départ pour l'atelier; elle ne constitue pas une autorisation de collecte automatisée. Le dossier `recherche` est volontairement local et exclu de GitHub.
