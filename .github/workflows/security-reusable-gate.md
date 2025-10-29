# 🛡️ Reusable Workflow — Security Gate

Ce workflow **Run Security Gate** agit comme un **point de contrôle de sécurité automatisé** dans vos pipelines CI/CD.  
Il orchestre deux analyses principales :
- **SAST** (Static Application Security Testing) : analyse du code source avec Semgrep.
- **SCA** (Software Composition Analysis) : détection des vulnérabilités dans les dépendances avec Trivy.

---
#############################
## ⚙️ Définition du workflow
#############################

```yaml
name: Run security Gate

on:
  workflow_call

jobs:
  sast_scan:
    uses: naouaichRepo/all-reusables-workflows/.github/workflows/security-reusable-sast-scan.yml@v1

  sca_scan:
    uses: naouaichRepo/all-reusables-workflows/.github/workflows/security-reusable-sca-scan.yml@v1

🧩 Fonctionnement
	•	Le job sast_scan exécute le workflow SAST qui vérifie la sécurité du code source.
	•	Le job sca_scan exécute le workflow SCA qui audite les bibliothèques tierces et dépendances.

Ces deux jobs s’exécutent en parallèle, permettant un retour rapide sur les vulnérabilités détectées.

⸻

#############################
🚀 Comment l’utiliser
#############################

Pour déclencher ce Security Gate, créez un workflow appelant :


```

name: Run security Scan

on:
  schedule:
    - cron: '0 2 * * *'  # Exécution quotidienne à 2h du matin UTC
  pull_request:
    branches:
      - main

jobs:
  security_gate:
    uses: naouaichRepo/all-reusables-workflows/.github/workflows/security-reusable-gate.yml@v1

```

#############################
🚀 Comment Customiser
#############################

Pour déclencher ce Security Gate, créez un workflow appelant :


```

name: Run security Scan

on:
  schedule:
    - cron: '0 2 * * *'  # Exécution quotidienne à 2h du matin UTC
  pull_request:
    branches:
      - main

jobs:
  security_gate:
    uses: naouaichRepo/all-reusables-workflows/.github/workflows/security-reusable-gate.yml@v1
    with:
      security-level: "MAX"
      is-blocking: true
```


#############################
🧠 Bonnes pratiques
#############################

	•	Planifiez un scan régulier via schedule (ex: tous les jours à 2h du matin).
	•	Déclenchez le scan sur les pull requests critiques (branches main, develop, preprod).
	•	Surveillez les résultats dans l’onglet Code Scanning Alerts de GitHub.
	•	Bloquez le merge si des vulnérabilités critiques sont détectées.


🧾 Résumé

Type de scan	Outil utilisé	Objectif principal
SAST	Semgrep	Analyse du code source et détection de failles logiques
SCA	Trivy	Détection des vulnérabilités dans les dépendances (packages, libs)


🧰 Repository source : naouaichRepo/all-reusables-workflows
📦 Version actuelle : v1
🕓 Dernière mise à jour : Automatique via GitHub Actions Scheduler

