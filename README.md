# DREY'S ERP — première version visuelle (Django)

Monolithe modulaire Django, interface en français, base SQLite par défaut (compatible compte gratuit PythonAnywhere).

## Ce qui fonctionne réellement
Connexion/déconnexion (limitation des tentatives), utilisateurs, 17 rôles avec permissions modifiables (contrôlées côté serveur), société, catalogue de modules avec activation/désactivation et dépendances, tableau de bord alimenté par les données, recherche globale, notifications calculées (stock faible, factures échues), journal d'activité, **CRM** (prospects/clients/fournisseurs, historique, conversion), **Inventaire** (produits, mouvements, stock négatif configurable), **Ventes** (factures brouillon → validation avec numérotation et sortie de stock → paiements → annulation avec remise en stock, impression/PDF via le navigateur).

## Ce qui n'est PAS développé
Achats, Comptabilité SYSCOHADA, RH, Paie, Projets, Fabrication, Point de vente (affichés « Bientôt disponible »), devis/commandes, PDF serveur, MFA, intégrations MTN/Orange Money (le mode de paiement est seulement une étiquette), multi-devises, graphiques interactifs. Le taux de taxe par défaut est **0** : à paramétrer et faire valider par un professionnel camerounais.

## Déploiement sur PythonAnywhere (pas à pas)
1. **Files** → *Upload a file* : envoyez `dreys_erp.zip` dans votre dossier personnel.
2. Ouvrez une console **Bash** et exécutez :
   ```bash
   unzip dreys_erp.zip && cd dreys_erp
   mkvirtualenv --python=python3.12 dreys-venv      # adaptez à la version Python choisie pour l'app web
   pip install -r requirements.txt
   cp .env.example .env && nano .env
   ```
   Dans `.env` : générez une clé avec
   `python -c "from django.core.management.utils import get_random_secret_key as g; print(g())"`,
   puis renseignez `DJANGO_SECRET_KEY`, `DJANGO_ALLOWED_HOSTS=VOTRENOM.pythonanywhere.com` et `DJANGO_CSRF_TRUSTED_ORIGINS=https://VOTRENOM.pythonanywhere.com`.
3. Initialisation :
   ```bash
   python manage.py migrate
   python manage.py init_erp
   python manage.py createsuperuser
   python manage.py seed_demo        # optionnel : données DÉMO (supprimables avec --purge)
   python manage.py collectstatic --noinput
   ```
4. Onglet **Web** → *Add a new web app* → **Manual configuration** (même version de Python que le virtualenv).
   - *Source code* et *Working directory* : `/home/VOTRENOM/dreys_erp`
   - *Virtualenv* : `/home/VOTRENOM/.virtualenvs/dreys-venv`
   - *Static files* : URL `/static/` → dossier `/home/VOTRENOM/dreys_erp/staticfiles`
   - Fichier **WSGI** : remplacez son contenu par :
     ```python
     import os, sys
     path = '/home/VOTRENOM/dreys_erp'
     if path not in sys.path:
         sys.path.insert(0, path)
     os.environ['DJANGO_SETTINGS_MODULE'] = 'config.settings'
     from django.core.wsgi import get_wsgi_application
     application = get_wsgi_application()
     ```
5. Cliquez sur **Reload**, puis ouvrez `https://VOTRENOM.pythonanywhere.com`.

## En local
```bash
python -m venv venv && source venv/bin/activate && pip install -r requirements.txt
export DJANGO_DEBUG=1
python manage.py migrate && python manage.py init_erp && python manage.py createsuperuser
python manage.py runserver
python manage.py test apps
```

## Notes
- Comptes gratuits PythonAnywhere : SQLite uniquement (MySQL sur offre payante, PostgreSQL non proposé en standard). Le passage à PostgreSQL est prévu via `DB_ENGINE` pour un déploiement ultérieur.
- Ne publiez jamais `.env`. Le zip n'en contient pas.
- Administration technique Django : `/gestion-technique/` (superutilisateurs uniquement).
