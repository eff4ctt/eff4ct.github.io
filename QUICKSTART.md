# 🚀 Guide de Démarrage Rapide

## Installation et déploiement en 5 minutes

### 1️⃣ Créer le repository GitHub

```bash
# Sur GitHub.com, créez un nouveau repository public
# Nom suggéré : votre-username.github.io ou cybersec-blog
```

### 2️⃣ Pousser le code

```bash
cd cybersec-blog
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/VOTRE-USERNAME/VOTRE-REPO.git
git push -u origin main
```

### 3️⃣ Activer GitHub Pages

1. Allez sur `https://github.com/VOTRE-USERNAME/VOTRE-REPO/settings/pages`
2. Sous "Source", sélectionnez **GitHub Actions**
3. Le déploiement démarre automatiquement !

### 4️⃣ Configurer le nom de domaine Namecheap

#### DNS Configuration (Namecheap)

Allez dans **Domain List > Manage > Advanced DNS** et ajoutez :

**Pour example.com :**
```
Type: A Record, Host: @, Value: 185.199.108.153
Type: A Record, Host: @, Value: 185.199.109.153
Type: A Record, Host: @, Value: 185.199.110.153
Type: A Record, Host: @, Value: 185.199.111.153
Type: CNAME, Host: www, Value: votre-username.github.io
```

**Pour blog.example.com :**
```
Type: CNAME, Host: blog, Value: votre-username.github.io
```

#### GitHub Configuration

1. Allez dans **Settings > Pages**
2. Dans "Custom domain", entrez votre domaine (example.com ou blog.example.com)
3. Cliquez sur **Save**
4. Attendez 5 minutes, puis cochez **Enforce HTTPS**

### 5️⃣ Mettre à jour la configuration

```bash
# Éditez hugo.toml et changez baseURL
baseURL = 'https://votre-domaine.com/'

# Créez le fichier CNAME
echo "votre-domaine.com" > static/CNAME

# Commitez et poussez
git add .
git commit -m "Configure custom domain"
git push
```

## ✅ Vérification

Attendez 2-5 minutes et visitez votre domaine !

## 📝 Créer votre premier article

```bash
# Créez un nouveau fichier dans content/posts/
# Par exemple : content/posts/mon-premier-writeup.md
```

```markdown
---
title: "Mon premier writeup"
date: 2025-10-29
draft: false
---

## Introduction

Contenu de votre writeup...
```

Puis :

```bash
git add content/posts/mon-premier-writeup.md
git commit -m "Add first writeup"
git push
```

Le site se met à jour automatiquement en 1-2 minutes !

## 🎨 Personnalisation

### Changer les informations personnelles

Éditez `hugo.toml` :
- `title` : Le titre de votre blog
- `author` : Votre nom
- `description` : Description courte
- `tagline` : Votre slogan

### Modifier la page About

Éditez `content/about/_index.md` avec vos infos.

### Changer le style

Modifiez `layouts/_default/baseof.html` pour changer les couleurs et le design.

## 🐛 Problèmes courants

**Le site ne s'affiche pas ?**
- Vérifiez l'onglet "Actions" sur GitHub pour voir si le déploiement a réussi
- Attendez 5-10 minutes après le premier push

**Le domaine personnalisé ne marche pas ?**
- Attendez jusqu'à 24h pour la propagation DNS
- Vérifiez que le fichier `static/CNAME` existe et contient votre domaine

**Erreur 404 ?**
- Vérifiez que `baseURL` dans `hugo.toml` correspond à votre domaine
- Assurez-vous que `draft: false` dans vos articles

## 📚 Ressources

- [README complet](README.md) - Documentation détaillée
- [Hugo Docs](https://gohugo.io/documentation/)
- [GitHub Pages](https://docs.github.com/en/pages)

---

**C'est tout ! Votre blog est en ligne ! 🎉**
