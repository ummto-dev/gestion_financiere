📘 Gestion Financière – Guide d’installation
🔹 Informations générales
App Name : gestion_financiere

Titre : Gestion Financière

Publisher : Cellule développement UMMTO

Frameworks :

Frappe v15.92.0

ERPNext v15.92.3

Pour mettre à jour les versions:
bench switch-to-branch v15.92.0 frappe erpnext && bench update --patch

🔹 Étapes d’installation
1. Cloner le dépôt
   
cd ~
git clone https://github.com/ummto-dev/gestion_financiere.git
cd gestion_financiere

3. Ajouter l’app au bench

cd ~/frappe-bench
bench get-app https://github.com/ummto-dev/gestion_financiere.git

4. Créer le site

bench new-site gestion.local

5. Restaurer la base de données

bench --site gestion.local restore apps/gestion_financiere/gestion_financiere/backups/20251222_142106-finance_local-database.sql.gz

7. Restaurer la configuration du site

cp apps/gestion_financiere/gestion_financiere/backups/20251222_142106-finance_local-site_config_backup.json sites/gestion.local/site_config.json

7. Installer l’app
   
bench --site gestion.local install-app gestion_financiere

9. Démarrer le site
    
bench start

👉 Accédez ensuite à http://gestion.local:8000 ou http://localhost:8000.

🔹 Vérification
Lister les apps installées :

bash
bench --site gestion.local list-apps
Vous devez voir :

frappe

erpnext

gestion_financiere

🔹 Structure du dépôt
Code
gestion_financiere/
├─ gestion_financiere/              # code source de l’app
├─ setup.py                         # fichier d’installation de l’app
├─ hooks.py                         # configuration Frappe
├─ modules.txt                      # déclaration des modules
├─ backups/
│  ├─ 20251222_142106-finance_local-database.sql.gz
│  └─ 20251222_142106-finance_local-site_config_backup.json
└─ README.md                        # ce guide
