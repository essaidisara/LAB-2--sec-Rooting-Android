
---

# TP – Rooting Android et Sécurité Mobile

# 1. Objectif du TP

L’objectif de ce TP est de comprendre le fonctionnement du rooting Android et ses impacts sur les mécanismes de sécurité Android.
Le travail a été réalisé dans un environnement AVD contrôlé afin d’observer le comportement d’une application face à un appareil rooté.

---

# 2. Fiche périmètre

| Élément     | Valeur                                                |
| ----------- | ----------------------------------------------------- |
| Application | OWASP UnCrackable Level 1                             |
| Support     | AVD Android Studio – Pixel 3 API 30                   |
| Objectif    | Comprendre le rooting Android et ses impacts sécurité |
| Données     | Fictives                                              |
| Réseau      | Test isolé                                            |

---

# 3. Vérification de l’AVD

## Commande exécutée

```bash id="z01j63"
adb devices
```

## Résultat obtenu

```bash id="e5mkxj"
List of devices attached
emulator-5554 device
```

## Interprétation

L’AVD Android est correctement détecté par ADB.

<img width="481" height="60" alt="image" src="https://github.com/user-attachments/assets/8a9cc5ee-655b-4666-b522-1ec114aba6b8" />


---

# 4. Rooting de l’AVD

## Commandes exécutées

```bash id="c3hcrr"
adb root
```
<img width="451" height="329" alt="image" src="https://github.com/user-attachments/assets/42e03cd9-ee9f-41bb-b999-0b40241ecd29" />

## Résultat

```bash id="tb7t85"
adbd is already running as root
```
<img width="466" height="77" alt="image" src="https://github.com/user-attachments/assets/06dfefc0-3765-4461-84b4-16cdb67df584" />

---

## Vérification des privilèges root

### Commande

```bash id="p0xgcn"
adb shell id
```

### Résultat

```bash id="m5rl7q"
uid=0(root) gid=0(root)
```

## Interprétation

Le shell Android dispose des privilèges root.

<img width="467" height="119" alt="image" src="https://github.com/user-attachments/assets/4b986d8f-ea03-45b4-8c0c-f6448ea201db" />


---

# 5. Vérification Verified Boot

## Commande

```bash id="n39jkh"
adb shell getprop ro.boot.verifiedbootstate
```

## Résultat obtenu

```bash id="j6pkr1"
```

(valeur vide)

## Interprétation

Certaines propriétés liées à Verified Boot ne sont pas exposées sur cet AVD.
Cela est fréquent dans les environnements d’émulation Android.

<img width="476" height="70" alt="image" src="https://github.com/user-attachments/assets/97909924-4c0a-4a08-8ab1-924771975a18" />


---

# 6. Définition du Rooting

Le rooting consiste à obtenir les privilèges super-utilisateur sur Android.
Ces privilèges permettent d’accéder aux zones protégées du système et de modifier son fonctionnement.
Le rooting est utile en laboratoire pour analyser certains comportements de sécurité.
Cependant, il réduit les garanties de sécurité du système et nécessite un environnement contrôlé.

---

# 7. Android Security Model

Android utilise un modèle de sécurité basé sur l’isolation des applications (sandboxing).
Chaque application fonctionne avec son propre identifiant utilisateur.
Le système de permissions contrôle l’accès aux ressources sensibles.
Android utilise également Verified Boot pour garantir l’intégrité du système.
Le rooting contourne certaines protections et réduit la confiance dans l’environnement.
Les applications sensibles peuvent donc détecter le root.

---

# 8. Verified Boot

## Objectif principal

Verified Boot garantit que le système Android démarré est authentique et n’a pas été modifié.

## Chain of Trust

Chaque composant du démarrage vérifie l’intégrité du composant suivant avant de lui faire confiance.

## Importance

Si le processus de démarrage est compromis, les protections de sécurité Android peuvent être contournées.

---

# 9. Android Verified Boot (AVB)

Android Verified Boot (AVB) ajoute des mécanismes avancés de vérification d’intégrité.
AVB protège également contre les attaques de rollback en empêchant l’installation de versions vulnérables du système.

---

# 10. Installation de l’application

## Commande utilisée

```bash id="t6x3t4"
adb install UnCrackable-Level1.apk
```

## Résultat

```bash id="26om3n"
Success
```
<img width="480" height="41" alt="image" src="https://github.com/user-attachments/assets/2c977a48-6e8e-4267-9011-1985827b1556" />

## Interprétation

L’application a été correctement installée dans l’AVD.

<img width="221" height="393" alt="image" src="https://github.com/user-attachments/assets/30e4a93e-4ed6-4aff-81d6-b79e28b0edcb" />

---

# 11. Test de l’application

## Observation réalisée

Lors du lancement de l’application, le message suivant apparaît :

```text id="3z9lfx"
Root detected!
This is unacceptable. The app is now going to exit.
```

## Interprétation

L’application détecte correctement l’environnement rooté et refuse de fonctionner.

📸 Capture à insérer :

* écran de l’application avec le message “Root detected!”

---

# 12. Scénarios réalisés

| Scénario | Description                     |
| -------- | ------------------------------- |
| 1        | Ouvrir l’application            |
| 2        | Observer la détection root      |
| 3        | Tester le comportement sécurité |

---

# 13. Intérêt du laboratoire rooté

Un environnement rooté permet :

* d’observer des artefacts système normalement inaccessibles
* d’analyser le comportement runtime d’une application
* de tester certaines protections sécurité

Ces tests doivent être réalisés uniquement dans un laboratoire autorisé.

---

# 14. Matrice de risques

1. Intégrité non garantie du système.
2. Surface d’attaque augmentée.
3. Risque d’exposition de données sensibles.
4. Instabilité du système.
5. Mélange possible de données personnelles et de test.
6. Persistance involontaire des données.
7. Risque réseau si environnement non isolé.
8. Traçabilité insuffisante des manipulations.

---

# 15. Mesures défensives

1. Utilisation d’un réseau isolé.
2. Utilisation de données fictives.
3. AVD dédié aux tests sécurité.
4. Réinitialisation de l’environnement après les tests.
5. Journalisation des manipulations.
6. Aucun compte personnel utilisé.
7. Contrôle des APK installées.
8. Conservation des preuves et captures.

---

# 16. OWASP MASVS

## STORAGE-1

Les données sensibles doivent être stockées de manière sécurisée.

## NETWORK-1

Les communications réseau doivent utiliser TLS correctement configuré.

---

# 17. OWASP MASTG

## Test 1

Vérifier la présence de données sensibles stockées en clair.

## Test 2

Analyser les logs Android avec adb logcat.

---

# 18. Sauvegarde des logs

## Commande utilisée

```powershell id="owqrb6"
adb logcat -d > logcat_root_check.txt
```
<img width="443" height="70" alt="image" src="https://github.com/user-attachments/assets/0ba2c9d9-d998-46ca-95a1-9a5a0a3260d8" />

## Résultat

Le fichier `logcat_root_check.txt` a été généré avec succès.

<img width="959" height="557" alt="image" src="https://github.com/user-attachments/assets/bc5150aa-cb0b-4c9f-90ea-7e763320ed3f" />


---

# 19. Remise à zéro de l’AVD

## Méthode utilisée

```text id="v7xv7n"
Android Studio → Device Manager → Wipe Data
```
<img width="464" height="263" alt="image" src="https://github.com/user-attachments/assets/03b5c831-8d07-479f-bff0-ad8da6564edc" />

## Résultat

L’AVD revient à son état initial.

## Importance

Cette opération supprime les données de test et garantit un environnement propre.


---

# 20. Checklist finale

```text id="fby0zs"
✓ Périmètre défini
✓ AVD propre utilisé
✓ Root obtenu
✓ Application installée
✓ Détection root observée
✓ Logs sauvegardés
✓ Captures réalisées
✓ Réinitialisation effectuée
✓ Aucun compte personnel utilisé
```

---

# 21. Conclusion

Ce TP a permis de comprendre les mécanismes de sécurité Android liés au rooting et à Verified Boot.
L’application OWASP UnCrackable Level 1 a détecté correctement l’environnement rooté et a refusé de fonctionner.
Le laboratoire a également montré l’importance de l’isolation, de la traçabilité et du nettoyage des environnements de test en cybersécurité mobile.
