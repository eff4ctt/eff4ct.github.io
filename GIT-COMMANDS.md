# 📋 Commandes Git - Aide-Mémoire

## Premier déploiement

```bash
# 1. Aller dans le dossier du projet
cd cybersec-blog

# 2. Personnaliser la configuration
# Éditez hugo.toml avec votre nom de domaine
# Éditez static/CNAME avec votre domaine
# Éditez content/about/_index.md avec vos infos

# 3. Initialiser Git
git init
git add .
git commit -m "Initial commit - Hugo cybersecurity blog"

# 4. Créer la branche main
git branch -M main

# 5. Ajouter le remote (changez l'URL avec la vôtre)
git remote add origin https://github.com/VOTRE-USERNAME/VOTRE-REPO.git

# 6. Pousser vers GitHub
git push -u origin main
```

## Workflow quotidien

### Créer un nouvel article

```bash
# Créer le fichier article
touch content/posts/mon-nouveau-writeup.md

# Ou copier un template existant
cp content/posts/writeup-xxe-moderne.md content/posts/mon-nouveau-writeup.md

# Éditer l'article avec votre éditeur préféré
nano content/posts/mon-nouveau-writeup.md
# ou
code content/posts/mon-nouveau-writeup.md
```

### Publier les modifications

```bash
# Voir les fichiers modifiés
git status

# Ajouter tous les fichiers modifiés
git add .

# Ou ajouter un fichier spécifique
git add content/posts/mon-nouveau-writeup.md

# Créer un commit avec un message descriptif
git commit -m "Add new writeup: XXE exploitation technique"

# Pousser vers GitHub
git push

# Le site se met à jour automatiquement en 1-2 minutes !
```

## Commandes Git utiles

### Voir l'historique

```bash
# Voir les derniers commits
git log

# Vue compacte
git log --oneline

# Avec le graphe
git log --graph --oneline --all
```

### Annuler des modifications

```bash
# Annuler les modifications d'un fichier (avant git add)
git checkout -- content/posts/article.md

# Enlever un fichier de la staging area (après git add, avant commit)
git reset HEAD content/posts/article.md

# Annuler le dernier commit (garde les modifications)
git reset --soft HEAD~1

# Annuler le dernier commit (supprime les modifications)
git reset --hard HEAD~1
```

### Branches

```bash
# Créer une nouvelle branche pour tester
git checkout -b test-nouveau-design

# Voir toutes les branches
git branch

# Changer de branche
git checkout main

# Fusionner une branche dans main
git checkout main
git merge test-nouveau-design

# Supprimer une branche
git branch -d test-nouveau-design
```

### Mettre à jour depuis GitHub

```bash
# Récupérer les dernières modifications
git pull
```

## Workflows avancés

### Corriger rapidement une typo après un push

```bash
# Faire la correction dans le fichier
nano content/posts/article.md

# Amender le dernier commit
git add content/posts/article.md
git commit --amend --no-edit

# Force push (⚠️ seulement si personne d'autre n'utilise le repo)
git push --force
```

### Travailler sur un brouillon

```bash
# Créer une branche pour le brouillon
git checkout -b draft/nouveau-writeup

# Créer l'article avec draft: true
echo '---
title: "Work in progress"
date: 2025-10-29
draft: true
---

Contenu en cours...' > content/posts/wip.md

# Commiter sur la branche draft
git add .
git commit -m "WIP: nouveau writeup"
git push -u origin draft/nouveau-writeup

# Quand c'est prêt, merger dans main
git checkout main
# Changer draft: false dans le fichier
git merge draft/nouveau-writeup
git push
```

### Collaborer avec d'autres

```bash
# Ajouter un collaborateur
# 1. Sur GitHub : Settings > Collaborators > Add people

# Pour le collaborateur :
git clone https://github.com/VOTRE-USERNAME/VOTRE-REPO.git
cd VOTRE-REPO

# Créer une branche pour leur contribution
git checkout -b feature/nouvel-article

# Faire les modifications
# ...

# Pousser la branche
git push -u origin feature/nouvel-article

# Créer une Pull Request sur GitHub
```

## Configuration Git recommandée

```bash
# Configurer votre identité
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@example.com"

# Couleurs dans le terminal
git config --global color.ui auto

# Éditeur par défaut
git config --global core.editor "nano"
# ou
git config --global core.editor "code --wait"

# Voir toute la configuration
git config --list
```

## Sauvegardes et tags

### Créer des tags pour les versions importantes

```bash
# Créer un tag annoté
git tag -a v1.0 -m "Premier déploiement public"

# Pousser les tags vers GitHub
git push origin --tags

# Lister les tags
git tag

# Revenir à un tag spécifique
git checkout v1.0
```

## Urgence : Restaurer le site

### Si vous avez cassé quelque chose

```bash
# Revenir au dernier commit qui fonctionnait
git log  # Trouvez le hash du commit qui marchait

git reset --hard abc1234  # Remplacez abc1234 par le hash

git push --force  # ⚠️ Attention, ceci écrase l'historique

# Le site sera restauré à l'état du commit choisi
```

## Nettoyage

### Supprimer les fichiers inutiles

```bash
# Voir les fichiers non suivis
git clean -n

# Supprimer les fichiers non suivis
git clean -f

# Supprimer aussi les dossiers
git clean -fd
```

## .gitignore

Le fichier `.gitignore` est déjà configuré pour ignorer :
- Le dossier `/public/` (généré par Hugo)
- Les fichiers système (`.DS_Store`, etc.)
- Les fichiers d'éditeur

Pour ajouter des fichiers à ignorer :

```bash
echo "mon-fichier-secret.txt" >> .gitignore
git add .gitignore
git commit -m "Update gitignore"
```

## Astuces pro

### Aliases Git utiles

Ajoutez dans `~/.gitconfig` :

```ini
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    unstage = reset HEAD --
    last = log -1 HEAD
    visual = log --graph --oneline --all
```

Utilisation :
```bash
git st  # au lieu de git status
git co main  # au lieu de git checkout main
```

### Commit messages

Bonnes pratiques pour les messages de commit :

```bash
# ✅ Bon
git commit -m "Add writeup: SQL injection via time-based blind"
git commit -m "Fix typo in XXE article"
git commit -m "Update about page with new certifications"

# ❌ Mauvais
git commit -m "update"
git commit -m "fix"
git commit -m "changes"
```

## Support

En cas de problème avec Git :
- [Git Documentation](https://git-scm.com/doc)
- [GitHub Guides](https://guides.github.com/)
- [Oh Shit, Git!?!](https://ohshitgit.com/) - Guide pour les erreurs courantes

---

**Gardez ce fichier à portée de main pour vos opérations Git quotidiennes ! 📖**
