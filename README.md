# Blog Cybersécurité avec Hugo

Blog personnel de cybersécurité pour partager des writeups, analyses et recherches en sécurité informatique.

## 🚀 Déploiement sur GitHub Pages avec nom de domaine Namecheap

### Étape 1 : Configurer le repository GitHub

1. **Créer un nouveau repository sur GitHub** :
   - Nom du repository : `votre-username.github.io` (ou un autre nom si vous utilisez un domaine personnalisé)
   - Cochez "Public"
   - Ne pas initialiser avec README (nous avons déjà nos fichiers)

2. **Pousser votre code vers GitHub** :

```bash
cd /chemin/vers/cybersec-blog
git init
git add .
git commit -m "Initial commit - Hugo cybersecurity blog"
git branch -M main
git remote add origin https://github.com/votre-username/votre-repo.git
git push -u origin main
```

### Étape 2 : Activer GitHub Pages

1. Allez dans votre repository GitHub
2. Cliquez sur **Settings** (Paramètres)
3. Dans le menu de gauche, cliquez sur **Pages**
4. Sous **Source**, sélectionnez **GitHub Actions**

Le workflow GitHub Actions va automatiquement déployer votre site dès le prochain push !

### Étape 3 : Configurer votre nom de domaine Namecheap

#### A. Configuration DNS chez Namecheap

1. Connectez-vous à votre compte Namecheap
2. Allez dans **Domain List** et cliquez sur **Manage** pour votre domaine
3. Allez dans l'onglet **Advanced DNS**
4. Ajoutez les enregistrements DNS suivants :

**Pour un domaine apex (example.com)** :

| Type  | Host | Value               | TTL       |
|-------|------|---------------------|-----------|
| A     | @    | 185.199.108.153     | Automatic |
| A     | @    | 185.199.109.153     | Automatic |
| A     | @    | 185.199.110.153     | Automatic |
| A     | @    | 185.199.111.153     | Automatic |
| CNAME | www  | votre-username.github.io | Automatic |

**Pour un sous-domaine (blog.example.com)** :

| Type  | Host | Value               | TTL       |
|-------|------|---------------------|-----------|
| CNAME | blog | votre-username.github.io | Automatic |

5. Supprimez tous les autres enregistrements A ou CNAME qui pourraient entrer en conflit

#### B. Configuration sur GitHub

1. Retournez dans **Settings > Pages** de votre repository
2. Dans **Custom domain**, entrez votre nom de domaine (exemple : `blog.example.com` ou `example.com`)
3. Cliquez sur **Save**
4. Attendez quelques minutes, puis cochez **Enforce HTTPS**

⚠️ **Important** : La propagation DNS peut prendre jusqu'à 48h, mais généralement c'est effectif en quelques minutes.

### Étape 4 : Mettre à jour votre configuration Hugo

Modifiez le fichier `hugo.toml` avec votre vrai nom de domaine :

```toml
baseURL = 'https://votre-domaine.com/'
# ou
baseURL = 'https://blog.votre-domaine.com/'
```

Puis poussez les changements :

```bash
git add hugo.toml
git commit -m "Update baseURL with custom domain"
git push
```

### Étape 5 : Créer le fichier CNAME (optionnel mais recommandé)

Créez un fichier `static/CNAME` avec votre nom de domaine :

```bash
echo "votre-domaine.com" > static/CNAME
# ou
echo "blog.votre-domaine.com" > static/CNAME
```

Puis commitez et poussez :

```bash
git add static/CNAME
git commit -m "Add CNAME file"
git push
```

## 📝 Ajouter un nouvel article

Pour créer un nouvel article :

```bash
hugo new content/posts/nom-de-votre-article.md
```

Puis éditez le fichier créé dans `content/posts/` et modifiez `draft: true` en `draft: false` quand vous êtes prêt à publier.

Structure d'un article :

```markdown
---
title: "Titre de votre article"
date: 2025-10-29
draft: false
---

## Introduction

Votre contenu ici...
```

## 🛠️ Développement local

### Installation de Hugo

**Sur macOS** :
```bash
brew install hugo
```

**Sur Ubuntu/Debian** :
```bash
sudo snap install hugo
```

**Sur Windows** :
```bash
choco install hugo-extended
```

### Lancer le serveur de développement

```bash
hugo server -D
```

Visitez http://localhost:1313

## 📁 Structure du projet

```
cybersec-blog/
├── .github/
│   └── workflows/
│       └── hugo.yml          # GitHub Actions workflow
├── content/
│   ├── about/
│   │   └── _index.md         # Page À propos
│   └── posts/
│       ├── article-1.md      # Vos articles
│       └── article-2.md
├── layouts/
│   └── _default/
│       ├── baseof.html       # Template de base
│       ├── home.html         # Page d'accueil
│       ├── single.html       # Article individuel
│       └── list.html         # Liste d'articles
├── static/
│   └── CNAME                 # Fichier CNAME pour domaine personnalisé
├── hugo.toml                 # Configuration Hugo
├── .gitignore
└── README.md
```

## 🎨 Personnalisation

### Modifier les informations personnelles

Éditez `hugo.toml` :

```toml
title = 'Votre Nom - Cybersecurity Blog'

[params]
  author = 'Votre Nom'
  description = 'Cybersecurity Researcher & CTF Player'
  tagline = 'Writeups, Research & Security Analysis'
```

### Modifier la page About

Éditez `content/about/_index.md` avec vos informations personnelles.

### Personnaliser le design

Modifiez `layouts/_default/baseof.html` pour changer les couleurs, polices, etc.

## 🔍 SEO et Meta

Le blog inclut automatiquement :
- Meta descriptions
- Titles optimisés
- URLs propres
- Sitemap XML (généré automatiquement par Hugo)

## 📊 Analytics (optionnel)

Pour ajouter Google Analytics, ajoutez dans `hugo.toml` :

```toml
[services]
  [services.googleAnalytics]
    ID = 'G-XXXXXXXXXX'
```

## 🐛 Dépannage

### Le site ne se déploie pas

1. Vérifiez que le workflow GitHub Actions s'est bien exécuté dans l'onglet "Actions"
2. Vérifiez que GitHub Pages est activé dans Settings > Pages
3. Assurez-vous que votre repository est public (ou que vous avez GitHub Pro pour les repos privés)

### Le domaine personnalisé ne fonctionne pas

1. Vérifiez que les DNS sont correctement configurés avec `dig votre-domaine.com`
2. Attendez jusqu'à 48h pour la propagation DNS complète
3. Vérifiez que le fichier `CNAME` est présent dans le dossier `static/`
4. Assurez-vous que "Enforce HTTPS" est activé (après que les DNS soient propagés)

### Erreur 404 après déploiement

1. Vérifiez que `baseURL` dans `hugo.toml` correspond à votre domaine
2. Assurez-vous que le fichier `CNAME` contient le bon domaine
3. Attendez quelques minutes pour que GitHub Pages se mette à jour

## 📚 Ressources

- [Documentation Hugo](https://gohugo.io/documentation/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Namecheap DNS Configuration](https://www.namecheap.com/support/knowledgebase/article.aspx/9645/2208/how-do-i-link-my-domain-to-github-pages/)

## 📜 Licence

Ce template est libre d'utilisation. Personnalisez-le selon vos besoins !

## ✨ Auteur

Créé avec ❤️ pour la communauté cybersécurité francophone.

---

**Bon blogging ! 🚀**
