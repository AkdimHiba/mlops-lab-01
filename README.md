# MLOps Lab 01 — Churn Prediction

Projet pédagogique MLOps autour d'un modèle de prédiction du **churn** (départ de clients). Il illustre les étapes d'entraînement, d'évaluation, d'enregistrement de versions et de retour à une version précédente (*rollback*), ainsi que l'exposition d'une application avec FastAPI.

## Objectifs

- Entraîner et versionner un modèle de Machine Learning.
- Évaluer ses performances avec des métriques de classification, notamment le F1-score.
- Contrôler son enregistrement avec un seuil minimal de qualité.
- Restaurer une version précédente si nécessaire.
- Démarrer une API avec FastAPI et Uvicorn.

## Structure du projet

```text
mlops-lab-01/
├── data/          # Données
├── logs/          # Journaux d'exécution
├── models/        # Modèles sauvegardés
├── registry/      # Informations de versionnage des modèles
├── reports/       # Rapports d'évaluation
└── src/           # Scripts Python et API
```

Les dossiers et les commandes présentés correspondent à la structure et aux scripts utilisés dans le projet. Le comportement exact des scripts dépend de leur implémentation.

## Installation

Cloner le dépôt :

```powershell
git clone https://github.com/AkdimHiba/mlops-lab-01.git
cd mlops-lab-01
```

Créer et activer un environnement virtuel sous Windows PowerShell :

```powershell
py -m venv venv_mlops
.\venv_mlops\Scripts\Activate.ps1
```

Si un fichier `requirements.txt` est présent :

```powershell
python -m pip install -r requirements.txt
```

## Pipeline MLOps

### 1. Entraînement

```powershell
python src/train.py --version v2
```

Entraîne le modèle et indique la version `v2`.

### 2. Évaluation

```powershell
python src/evaluate.py
```

Évalue les performances du modèle. Le **F1-score** combine la précision (*precision*) et le rappel (*recall*) :

\[
F1 = 2 \times \frac{Precision \times Recall}{Precision + Recall}
\]

### 3. Enregistrement du modèle

```powershell
python src/register.py --gate-f1 0.70
```

Demande l'enregistrement selon un seuil de F1-score de **0,70**. Le comportement exact de la validation est défini dans `register.py`.

### 4. Rollback (si nécessaire)

```powershell
python src/rollback.py --target churn_model_b9ae3acc99174392a57ad4e8df787798.joblib
```

Sélectionne un modèle précédemment sauvegardé comme cible de restauration. Le fichier doit exister à l'emplacement attendu par le script. Le rollback n'est pas une étape obligatoire après chaque entraînement.

## Démarrer l'API

```powershell
uvicorn src.api:app --reload
```

- API locale : http://127.0.0.1:8000
- Documentation interactive FastAPI : http://127.0.0.1:8000/docs

**Remarque :** une réponse `404 Not Found` sur `/` est normale si aucune route d'accueil n'est définie.

## Concepts clés

| Concept | Rôle |
|---|---|
| **Training** | Apprendre à partir des données |
| **Evaluation** | Mesurer les performances du modèle |
| **F1-score** | Combiner précision et rappel |
| **Model registry** | Suivre les versions des modèles |
| **Quality gate** | Appliquer un seuil de performance |
| **Rollback** | Revenir à un modèle antérieur |
| **FastAPI** | Exposer des fonctionnalités via une API |
| **Uvicorn** | Exécuter le serveur ASGI |
| **Git / GitHub** | Versionner et partager le code |

## Bonnes pratiques

- Ne jamais publier de mots de passe, de clés API ou de fichiers `.env` contenant des secrets.
- Exclure l'environnement virtuel et les fichiers temporaires avec `.gitignore`.
- Vérifier les fichiers suivis avant de faire un `git push`.
- Documenter les métriques et la version du modèle utilisé.

## Dépôt GitHub

https://github.com/AkdimHiba/mlops-lab-01
