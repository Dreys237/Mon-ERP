# PROJECT_STATUS

Version : 0.1 (première version visuelle) — 23/11/2026

| Domaine | État |
|---|---|
| Authentification, rôles, permissions serveur, utilisateurs | Opérationnel, testé |
| Sociétés (modèle + isolation des données par société) | Opérationnel (interface de création de sociétés : à faire) |
| Catalogue de modules + dépendances | Opérationnel, testé |
| Tableau de bord, recherche, notifications, journal | Opérationnel |
| CRM (contacts, historique, conversion) | Opérationnel — pipeline/opportunités : à faire |
| Inventaire (produits, mouvements) | Opérationnel — entrepôts multiples, lots, inventaires physiques : à faire |
| Ventes (factures, paiements, annulation) | Opérationnel — devis, commandes, avoirs, PDF serveur : à faire |
| Comptabilité SYSCOHADA, Achats, RH, Paie, Projets, Fabrication, POS | Non développés |
| MFA, intégrations Mobile Money, multi-devises | Non développés |

Tests : 19 tests automatisés (authentification, permissions, isolation, calculs Decimal, stock, paiements). Playwright : à faire.
Données de démonstration : uniquement via `seed_demo`, marquées DÉMO, supprimables par `seed_demo --purge`.
Réglementation : aucun taux fiscal/social n'est préconfiguré (taxe par défaut = 0).
