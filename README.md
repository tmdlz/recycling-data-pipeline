🔄 Paprec Data Pipeline
Pipeline de données automatisé pour la collecte et la validation des tonnages de déchets — construit avec une approche DevOps complète.
Hébergé via Repos sur Azure.

📋 Contexte
Paprec Group (350 sites, 16M tonnes/an) génère quotidiennement des rapports CSV de collecte par site. Ce projet automatise l'ingestion, la validation et le chargement de ces données, avec une infrastructure provisionnée en code et un pipeline CI/CD multi-stage.

🏗️ Architecture

 CSV (collecte sites)
        │
        ▼
  ┌─────────────┐     ┌──────────────┐
  │  ingest.py   │────▶│  validate.py  │
  └─────────────┘     └──────┬───────┘
                             │
                             ▼
                    ┌──────────────┐     ┌─────────────┐
                    │ transform.py  │────▶│   load.py    │
                    └──────────────┘     └──────┬──────┘
                                                │
                                                ▼
                                          SQLite DB
🛠️ Stack technique
Composant	Outil	Rôle
IaC	Bicep	Provisionnement Azure (RG, Storage, Key Vault)
CI/CD	Azure Pipelines (YAML)	Build, lint, tests, déploiement
Data pipeline	Python 3.12	Ingestion, validation, transformation
Tests	pytest + flake8	Tests data + linting
Secrets	Azure Key Vault	Gestion sécurisée des secrets (RBAC)
Backlog	Azure Boards	User Stories, traçabilité story → PR → release
📁 Structure du repo

 paprec-data-pipeline/
├── docs/                        # Documentation projet
│   ├── PROJECT_BRIEF.md         # Brief projet (contexte, outils, approche)
│   └── RUNBOOK.md               # Guide d'exploitation
├── infra/                       # Infrastructure as Code
│   ├── main.bicep               # Template Bicep principal
│   └── parameters/
│       ├── dev.bicepparam       # Paramètres env dev
│       └── prod.bicepparam      # Paramètres env prod
├── src/                         # Code applicatif
│   ├── ingest.py                # Ingestion des CSV
│   ├── validate.py              # Validation qualité données
│   ├── transform.py             # Transformation / nettoyage
│   └── load.py                  # Chargement SQLite
├── tests/                       # Tests automatisés
│   ├── test_validate.py         # Tests de validation
│   ├── test_transform.py        # Tests de transformation
│   └── sample_data/             # Jeux de données de test
├── azure-pipelines.yml          # Pipeline CI/CD multi-stage
├── requirements.txt             # Dépendances Python
├── .flake8                      # Config linting
└── .gitignore
🚀 Démarrage rapide
Prérequis

Python 3.12+
Azure CLI + extension Bicep
Un compte Azure avec un abonnement actif
Installation locale


 git clone https://github.com/tmdlz/paprec-data-pipeline.git
cd paprec-data-pipeline
pip install -r requirements.txt
Lancer les tests


 pytest tests/ -v
flake8 src/
Déployer l'infra (dev)


 az group create --name rg-paprec-dev --location francecentral
az deployment group create \
  --resource-group rg-paprec-dev \
  --template-file infra/main.bicep \
  --parameters infra/parameters/dev.bicepparam
📊 Métriques pipeline
Métrique	Cible
Temps de build CI	< 2 min
Couverture tests data	100% des règles métier
Lint errors	0
Déploiement IaC	Idempotent, reproductible
📝 Workflow de contribution
Créer une branche feature/US-XX-description depuis develop
Développer + tester en local
Ouvrir une PR vers develop (utiliser le template)
CI passe → review → merge
Release : merge develop → main
📄 Licence
Projet de démonstration — usage personnel et pédagogique.
