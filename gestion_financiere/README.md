📘 Gestion Financière – Guide d’installation
🔹 Informations générales
•	App Name : gestion_financiere
•	Titre : Gestion Financière
•	Publisher : Cellule Développement UMMTO
Frameworks utilisés :
•	Frappe v15.92.0
•	ERPNext v15.92.3
👉 Pour mettre à jour les versions :
Bash

cd apps/frappe
git fetch upstream
git checkout v15.92.0

cd apps/erpnext
git fetch upstream
git checkout v15.92.3


	Revenir dans ton bench et lancer la mise à jour

bash
cd ~/my-frappe-bench
bench update –patch

	Recompiler les assets
bash
bench build

	Redémarrer ton bench
bash
bench restart

	En cas de probléme avec detached HEAD :

Étapes pour finaliser la mise à jour

1.	Créer une branche locale à partir du tag
bash
git switch -c frappe-v15.92.0
👉 Ça va créer une branche frappe-v15.92.0 pointant sur le tag v15.92.0
2.	Faire la même chose pour ERPNext
bash
cd ../erpnext
git fetch upstream
git checkout v15.92.3
git switch -c erpnext-v15.92.3

3.	Revenir dans ton bench et appliquer la mise à jour
bash
cd ~/my-frappe-bench
bench update –patch

4.	Recompiler les assets
bash
bench build

5.	Redémarrer ton bench
bash
bench restart

🔎 Vérifier les versions installées

1.	Depuis ton dossier my-frappe-bench :
bash
cd ~/my-frappe-bench

2.	Vérifier toutes les apps installées et leurs versions :
bash
bench version

👉 Tu verras un tableau du style :

erpnext 15.92.3
frappe  15.92.0
🔹 Étapes d’installation
1.	 Ajouter l’app au bench
bash
cd ~/frappe-bench
bench get-app gestion_financiere https://github.com/ummto-dev/gestion_financiere.git

2.	Créer un utilisateur dédié pour Bench

1.	Connecte-toi à MariaDB avec socket
bash
sudo mysql
2.	Crée un utilisateur dédié (frappe)

CREATE USER 'frappe'@'localhost' IDENTIFIED BY 'erpdb@25';
GRANT ALL PRIVILEGES ON *.* TO 'frappe'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;


👉 Ici tu donnes tous les droits à frappe, ce qui permet à Bench de créer/droper des bases sans passer par root.

🔧 Utiliser cet utilisateur dans Bench

1.	Mets à jour ton common_site_config.json :

bash
nano ~/my-frappe-bench/sites/common_site_config.json

	Ajoute/modifie :

{
  "db_host": "localhost",
  "db_port": 3306,
  "db_name": "gestion_financiere_db",
  "db_user": "frappe",
  "db_password": "erpdb@25"
}

2. Crée le site
bash
bench new-site finance.local --db-name gestion_financiere_db --db-root-username frappe --db-root-password erpdb@25
	voir les applications installées sur ton site finance.local :
cd ~/my-frappe-bench
bench --site finance.local list-apps
	Installe ERPNext sur ce site
bench get-app erpnext --branch v15.92.3 https://github.com/frappe/erpnext.git
bench --site finance.local install-app erpnext

	Installe ton app personnalisée (gestion_financiere)
bash
bench --site finance.local install-app gestion_financiere
	Recompiler et redémarrer
bash
bench build
bench restart
	Vérification
bash
bench --site finance.local list-apps
👉 Résultat attendu :
Code
frappe             15.92.0
erpnext            15.92.3
gestion_financiere 0.0.1

6. Démarrer le site
bash
bench start
👉 Accédez ensuite à :
•	http://finance.local:8000
•	ou http://localhost:8000
🔧 Comment exporter un Doctype
1. Fixtures (export via hooks.py)
•	Si tu ajoutes ton Doctype dans fixtures :
fixtures = ["Faculte"]

•	Puis tu fais :
bash
bench --site finance.local export-fixtures
👉 Cela génère un fichier JSON (faculte.json) qui contient la définition du Doctype ET les enregistrements existants.
apps/gestion_financiere/gestion_financiere/fixtures/faculte.json
	Ajoute ce fichier à ton repo Git :
bash
cd ~/my-frappe-bench/apps/gestion_financiere
git add gestion_financiere/fixtures/faculte.json
git commit -m "Ajout du Doctype Faculte avec données"
git push
🔧 Étapes pour mettre à jour le dépôt
cd ~/my-frappe-bench/apps/gestion_financiere
git pull origin master
	Appliquer les fixtures
Les fixtures (comme ton faculte.json) sont automatiquement importées lors d’une migration :
bash
bench --site finance.local migrate
👉 Cela recrée le Doctype Faculte et insère les données exportées (ex. Faculté des Sciences).
	. Redémarrer le bench
bash
bench restart
