## Exercice 1 - Conception Logicielle

### 1. Diagramme de contexte

Le système DevOpsGPT communique avec trois éléments principaux :

- L'utilisateur : il envoie des questions et reçoit des réponses.
- L'API GPT-4 : elle génère une réponse à partir du message envoyé.
- La base de données : elle sauvegarde l'historique des messages et des réponses.

### 2. Flowchart - Fonctionnement d'un message

Étapes de fonctionnement :

1. L'utilisateur envoie un message.
2. Le système DevOpsGPT reçoit le message.
3. Le système vérifie si le message contient des insultes.
4. Si le message contient des insultes, le message est bloqué.
5. Si le message ne contient pas d'insultes, il est envoyé à l'API GPT-4.
6. L'API GPT-4 génère une réponse.
7. La réponse est sauvegardée dans la base de données.
8. La réponse est affichée à l'utilisateur.

Voir les fichiers :

- diagramme-contexte-devopsgpt.png
- flowchart-message-devopsgpt.png

### 3. Dictionnaire de données

Voir le fichier :

- dictionnaire-donnees-message.png

## Exercice 2 - Git et Docker

### 1. Méthodologie et Git

#### Question A - User Story

En tant qu'utilisateur,
je veux souscrire à un abonnement premium,
afin d'accéder à des fonctionnalités avancées et à une meilleure expérience utilisateur.

#### Question B - Commandes Git

Créer une branche feature et faire un commit :

```bash
git checkout -b feature-premium-subscription

git add .

git commit -m "Ajout du système d'abonnement premium"
```

Créer un tag de version après fusion :

```bash
git tag v1.0.0
```

Pousser le tag sur GitHub :

```bash
git push origin v1.0.0
```

## Exercice 3 - CI/CD avec GitHub Actions

### 1. Workflow CI/CD

Le workflow GitHub Actions a été créé dans :

```text
.github/workflows/main.yml
```

Fonctionnement :

- Déclenchement lors d'un push sur la branche `main`.
- Déclenchement lors de la création d'un tag commençant par `v`.
- Exécution du job `test-and-deploy` sur `ubuntu-latest`.

Étapes exécutées :

1. Récupération du code avec `actions/checkout@v4`.
2. Installation de Node.js version 22 avec `actions/setup-node@v4`.
3. Installation des dépendances avec `npm install`.
4. Exécution des tests avec `npm test`.
5. Déploiement simulé avec :

```bash
echo "Déploiement en cours..."
```

Cette étape ne s'exécute que lors d'un push d'un tag de version.

### 2. Sécurité et Secrets

#### Question A

Pour enregistrer la clé `OPENAI_API_KEY` de manière sécurisée sur GitHub :

1. Aller sur le dépôt GitHub.
2. Cliquer sur `Settings`.
3. Aller dans `Secrets and variables`.
4. Cliquer sur `Actions`.
5. Cliquer sur `New repository secret`.
6. Dans `Name`, écrire `OPENAI_API_KEY`.
7. Dans `Secret`, coller la clé API.
8. Cliquer sur `Add secret`.

La clé n'est donc jamais écrite en clair dans le code.

#### Question B

Pour injecter le secret dans le fichier `.github/workflows/main.yml`, on utilise la syntaxe suivante :

```yaml
env:
  OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
```

Exemple dans une étape de déploiement :

```yaml
- name: Déploiement
  if: startsWith(github.ref, 'refs/tags/v')
  env:
    OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
  run: echo "Déploiement en cours..."
```