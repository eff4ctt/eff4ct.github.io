# 🎯 Blog Cybersécurité Hugo - Vue d'ensemble

Bienvenue dans votre nouveau blog de cybersécurité ! Ce projet est prêt à être déployé sur GitHub Pages avec votre nom de domaine Namecheap.

## 📁 Structure du projet

```
cybersec-blog/
│
├── 📘 QUICKSTART.md           ← COMMENCEZ ICI ! Guide rapide 5 minutes
├── 📗 README.md               ← Documentation complète
├── 📙 NAMECHEAP-GUIDE.md      ← Configuration DNS détaillée
├── 📕 GIT-COMMANDS.md         ← Aide-mémoire Git
│
├── .github/workflows/
│   └── hugo.yml               ← Déploiement automatique GitHub Actions
│
├── content/
│   ├── about/_index.md        ← Votre page "À propos"
│   └── posts/                 ← VOS ARTICLES ICI
│       ├── writeup-xxe-moderne.md
│       ├── sql-injection-time-based.md
│       └── buffer-overflow-arm.md
│
├── layouts/_default/
│   ├── baseof.html           ← Template de base (design)
│   ├── home.html             ← Page d'accueil
│   ├── single.html           ← Page article
│   └── list.html             ← Liste d'articles
│
├── static/
│   └── CNAME                 ← Votre nom de domaine
│
├── hugo.toml                 ← Configuration Hugo
└── .gitignore                ← Fichiers à ignorer
```

## 🚀 Démarrage rapide (3 étapes)

### Étape 1 : Créer un repository GitHub
- Nom suggéré : `votre-username.github.io` ou `cybersec-blog`
- Visibilité : Public

### Étape 2 : Pousser le code
```bash
cd cybersec-blog
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/VOTRE-USERNAME/VOTRE-REPO.git
git push -u origin main
```

### Étape 3 : Activer GitHub Pages
1. Allez dans Settings > Pages
2. Source : **GitHub Actions**
3. Votre site sera déployé automatiquement !

**📖 Pour plus de détails, consultez [QUICKSTART.md](QUICKSTART.md)**

## 🌐 Configuration du nom de domaine

### Sur Namecheap (Advanced DNS)

**Pour votre-domaine.com :**
```
Type: A,     Host: @,   Value: 185.199.108.153
Type: A,     Host: @,   Value: 185.199.109.153
Type: A,     Host: @,   Value: 185.199.110.153
Type: A,     Host: @,   Value: 185.199.111.153
Type: CNAME, Host: www, Value: votre-username.github.io
```

**Pour blog.votre-domaine.com :**
```
Type: CNAME, Host: blog, Value: votre-username.github.io
```

### Sur GitHub (Settings > Pages)
1. Custom domain : `votre-domaine.com` (ou `blog.votre-domaine.com`)
2. Attendez 5-10 minutes
3. Cochez "Enforce HTTPS"

**📖 Guide détaillé : [NAMECHEAP-GUIDE.md](NAMECHEAP-GUIDE.md)**

## ✏️ Personnalisation

### 1. Informations personnelles

Éditez `hugo.toml` :
```toml
baseURL = 'https://votre-domaine.com/'
title = 'Votre Nom - Cybersecurity Blog'

[params]
  author = 'Votre Nom'
  description = 'Cybersecurity Researcher & CTF Player'
```

### 2. Nom de domaine

Éditez `static/CNAME` :
```
votre-domaine.com
```

### 3. Page À propos

Éditez `content/about/_index.md` avec vos infos, certifications, contact, etc.

### 4. Créer un article

```bash
# Créez un nouveau fichier
touch content/posts/mon-writeup.md
```

Structure d'un article :
```markdown
---
title: "Mon nouveau writeup"
date: 2025-10-29
draft: false
---

## Introduction

Votre contenu ici...

## Exploitation

\`\`\`python
# Votre code
\`\`\`

## Conclusion
```

Puis :
```bash
git add content/posts/mon-writeup.md
git commit -m "Add new writeup"
git push
```

Le site se met à jour automatiquement en 1-2 minutes !

## 🎨 Design

Le blog utilise un design minimaliste inspiré d'Orange Tsai :
- ✅ Police sans-serif moderne et lisible
- ✅ Mise en page sobre et professionnelle
- ✅ Coloration syntaxique pour le code
- ✅ Responsive (mobile-friendly)
- ✅ Mode sombre automatique du navigateur

Pour personnaliser le design, éditez `layouts/_default/baseof.html`.

## 📝 Exemples d'articles inclus

Le projet contient 3 articles exemples :
1. **Exploitation XXE moderne** - Writeup web security
2. **SQL Injection time-based** - Bug bounty success story
3. **Buffer Overflow ARM** - Binary exploitation CTF

Ces articles servent de template. Vous pouvez :
- Les modifier
- Les supprimer
- Les utiliser comme base pour vos propres writeups

## 🔧 Commandes Git essentielles

```bash
# Voir les modifications
git status

# Ajouter tous les fichiers
git add .

# Créer un commit
git commit -m "Votre message"

# Pousser vers GitHub
git push

# Voir l'historique
git log --oneline
```

**📖 Guide complet : [GIT-COMMANDS.md](GIT-COMMANDS.md)**

## ✅ Checklist de déploiement

- [ ] Repository GitHub créé
- [ ] Code poussé sur GitHub
- [ ] GitHub Pages activé (GitHub Actions)
- [ ] Nom de domaine acheté sur Namecheap (vous avez 1 an gratuit)
- [ ] DNS configuré sur Namecheap
- [ ] Domaine personnalisé ajouté sur GitHub
- [ ] Fichier `static/CNAME` créé
- [ ] `baseURL` dans `hugo.toml` mis à jour
- [ ] Page About personnalisée
- [ ] Workflow GitHub Actions réussi (vérifiez l'onglet Actions)
- [ ] Site accessible via votre nom de domaine (peut prendre 1-24h)
- [ ] HTTPS activé sur GitHub Pages

## 🐛 Problèmes courants

### Le site ne se déploie pas
- Vérifiez l'onglet "Actions" sur GitHub
- Assurez-vous que GitHub Pages est activé (Source: GitHub Actions)

### Le domaine ne fonctionne pas
- Attendez 1-24h pour la propagation DNS
- Vérifiez que `static/CNAME` contient le bon domaine
- Utilisez `dig votre-domaine.com` pour vérifier les DNS

### Erreur 404 sur les articles
- Vérifiez que `draft: false` dans vos articles
- Vérifiez que `baseURL` dans `hugo.toml` est correct

### Le style ne s'affiche pas
- Le `baseURL` dans `hugo.toml` doit correspondre exactement à votre domaine
- Incluez le `https://` et le `/` final

## 📚 Documentation

| Fichier | Description |
|---------|-------------|
| [QUICKSTART.md](QUICKSTART.md) | Guide de démarrage rapide (5 min) |
| [README.md](README.md) | Documentation complète du projet |
| [NAMECHEAP-GUIDE.md](NAMECHEAP-GUIDE.md) | Configuration DNS Namecheap |
| [GIT-COMMANDS.md](GIT-COMMANDS.md) | Commandes Git pour gérer le blog |

## 🆘 Besoin d'aide ?

1. **Documentation Hugo** : https://gohugo.io/documentation/
2. **GitHub Pages** : https://docs.github.com/en/pages
3. **Namecheap Support** : https://www.namecheap.com/support/
4. **Markdown Guide** : https://www.markdownguide.org/

## 🎯 Prochaines étapes

1. ✅ Lisez [QUICKSTART.md](QUICKSTART.md)
2. ✅ Personnalisez `hugo.toml` et `content/about/_index.md`
3. ✅ Configurez votre nom de domaine (suivez [NAMECHEAP-GUIDE.md](NAMECHEAP-GUIDE.md))
4. ✅ Poussez sur GitHub
5. ✅ Écrivez votre premier writeup !

## 📦 Contenu du package

Vous avez téléchargé :
- ✅ Blog Hugo complet et fonctionnel
- ✅ Design sobre inspiré d'Orange Tsai
- ✅ 3 articles exemples
- ✅ Déploiement automatique GitHub Actions
- ✅ Configuration GitHub Pages
- ✅ Guides de configuration Namecheap
- ✅ Documentation complète

## 💡 Conseils

- Écrivez régulièrement pour améliorer votre visibilité
- Partagez vos articles sur Twitter avec #cybersecurity #infosec
- Participez aux CTF et documentez vos exploits
- Créez des articles de qualité plutôt que de la quantité
- Utilisez des captures d'écran et du code pour illustrer

## 🏆 Exemples de contenu

Idées d'articles pour votre blog :
- Writeups de CTF
- Bug bounty findings (après disclosure)
- Analyses de vulnérabilités
- Guides techniques (reverse engineering, web hacking, etc.)
- Outils de pentest
- Configuration de lab de sécurité
- Retours d'expérience sur des certifications

---

## 🚀 Prêt ? Commencez maintenant !

**Ouvrez [QUICKSTART.md](QUICKSTART.md) et suivez les instructions en 5 minutes.**

Votre blog sera en ligne aujourd'hui ! 🎉

---

*Créé avec ❤️ pour la communauté cybersécurité francophone*
*Bon blogging ! 🔐*
