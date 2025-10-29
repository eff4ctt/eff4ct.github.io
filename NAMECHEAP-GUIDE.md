# 🌐 Guide Configuration Namecheap - Étape par Étape

## Configuration DNS pour GitHub Pages

### Option 1 : Domaine Principal (example.com)

#### Étape 1 : Accéder aux paramètres DNS

1. Connectez-vous à [Namecheap](https://www.namecheap.com)
2. Cliquez sur **Domain List** dans le menu
3. Trouvez votre domaine et cliquez sur **Manage**
4. Cliquez sur l'onglet **Advanced DNS**

#### Étape 2 : Supprimer les enregistrements existants

Supprimez tous les enregistrements A et CNAME existants pour éviter les conflits.

#### Étape 3 : Ajouter les nouveaux enregistrements DNS

Cliquez sur **Add New Record** et ajoutez ces 5 enregistrements :

| Type | Host | Value | TTL |
|------|------|-------|-----|
| A Record | @ | 185.199.108.153 | Automatic |
| A Record | @ | 185.199.109.153 | Automatic |
| A Record | @ | 185.199.110.153 | Automatic |
| A Record | @ | 185.199.111.153 | Automatic |
| CNAME Record | www | votre-username.github.io. | Automatic |

⚠️ **Important** : Remplacez `votre-username` par votre nom d'utilisateur GitHub réel !

#### Étape 4 : Sauvegarder

Cliquez sur le bouton ✓ (coche verte) pour chaque enregistrement.

---

### Option 2 : Sous-domaine (blog.example.com)

#### Configuration DNS plus simple

Si vous préférez utiliser un sous-domaine comme `blog.example.com` :

| Type | Host | Value | TTL |
|------|------|-------|-----|
| CNAME Record | blog | votre-username.github.io. | Automatic |

C'est tout ! Un seul enregistrement suffit pour un sous-domaine.

---

## Configuration GitHub

### Étape 1 : Aller dans les paramètres Pages

```
https://github.com/votre-username/votre-repo/settings/pages
```

### Étape 2 : Ajouter le domaine personnalisé

1. Dans la section **Custom domain**, entrez votre domaine :
   - Pour domaine principal : `example.com`
   - Pour sous-domaine : `blog.example.com`

2. Cliquez sur **Save**

3. GitHub va vérifier le DNS (cela peut prendre quelques minutes)

### Étape 3 : Activer HTTPS

⏱️ Attendez 5-10 minutes après que "DNS check successful" apparaisse

Ensuite, cochez la case **Enforce HTTPS**

---

## ⏰ Temps de propagation

- **Minimum** : 5-10 minutes
- **Typique** : 1-2 heures
- **Maximum** : 24-48 heures

### Comment vérifier la propagation DNS ?

```bash
# Pour un domaine principal
dig example.com

# Pour un sous-domaine
dig blog.example.com

# Vous devriez voir les adresses IP GitHub dans les résultats
```

Ou utilisez un outil en ligne : [whatsmydns.net](https://www.whatsmydns.net/)

---

## ✅ Checklist finale

- [ ] DNS configuré sur Namecheap
- [ ] Domaine ajouté dans GitHub Pages settings
- [ ] Fichier `static/CNAME` créé avec le bon domaine
- [ ] `baseURL` dans `hugo.toml` mis à jour
- [ ] Code poussé sur GitHub
- [ ] Workflow GitHub Actions réussi (onglet Actions)
- [ ] Attendu 10+ minutes pour propagation DNS
- [ ] HTTPS activé

---

## 🐛 Dépannage

### "DNS check failed" sur GitHub

**Cause** : Les DNS ne sont pas encore propagés

**Solution** : Attendez 1-2 heures et réessayez

### "There isn't a GitHub Pages site here"

**Causes possibles** :
1. Le workflow GitHub Actions n'a pas réussi
2. Le fichier CNAME n'est pas dans `static/`
3. Le baseURL dans hugo.toml ne correspond pas au domaine

**Solutions** :
1. Vérifiez l'onglet "Actions" sur GitHub
2. Vérifiez que `static/CNAME` existe et contient votre domaine
3. Mettez à jour `hugo.toml` avec le bon baseURL

### Le site affiche le contenu mais sans style

**Cause** : Le baseURL ne correspond pas au domaine réel

**Solution** :
```toml
# Dans hugo.toml, assurez-vous que baseURL correspond exactement à votre domaine
baseURL = 'https://example.com/'  # ou https://blog.example.com/
```

### HTTPS non disponible

**Cause** : Il faut attendre que Let's Encrypt génère le certificat

**Solution** : Attendez 5-10 minutes après la vérification DNS réussie

---

## 💡 Conseils

### Domaine principal vs Sous-domaine

**Domaine principal (example.com)** :
- ✅ Plus professionnel
- ✅ Meilleur pour le SEO
- ❌ Configuration DNS plus complexe (4 enregistrements A)

**Sous-domaine (blog.example.com)** :
- ✅ Configuration plus simple (1 CNAME)
- ✅ Permet de garder example.com pour autre chose
- ✅ Plus flexible
- ❌ URL légèrement plus longue

### Redirection www

Si vous utilisez le domaine principal (example.com), les visiteurs qui tapent www.example.com seront automatiquement redirigés vers example.com grâce au CNAME www que nous avons configuré.

---

## 📞 Support

Si vous rencontrez des problèmes :

1. **Namecheap Support** : [Live Chat 24/7](https://www.namecheap.com/support/)
2. **GitHub Support** : [GitHub Community](https://github.community/)
3. **Documentation Hugo** : [gohugo.io](https://gohugo.io/documentation/)

---

**Votre blog sera en ligne sous quelques heures maximum ! 🎉**
