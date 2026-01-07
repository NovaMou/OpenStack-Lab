# OpenStack-Lab
Projet OpenStack - gestion de VM et ressources Cloud

## Objectif du projet
Installer et configurer OpenStack (MicroStack) sur Ubuntu avec Snap et lancer une instance de machine virtuelle depuis le tableau de bord OpenStack ou via le CLI.

## Prérequis
- Ubuntu 20.04 ou plus récent (physique, VM)
- ≥ 8 Go RAM, ≥ 50 Go disque
- Connaissances de base du terminal Linux

## Étape 1 – Installation d’OpenStack (MicroStack)
![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/e8411c49884ca809ccabb6c961b4e818eeba5805/linuxcommande.webp)

Mettre à jour le système :

```console
busa@busa:~$ sudo apt update && sudo apt upgrade -y
```
Installer MicroStack :
```console
busa@busa:~$ sudo snap install microstack --classic
```
Microstack --classic n'est pas disponible, mais --edge est disponible.
Exécuter la commande :
```console
busa@busa:~$ sudo snap install microstack --classic --edge
```
![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/e7006853c5ab7a48c60b33691629810877b589f8/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_peq81bbsearcbqht0htt.webp)

Initialiser MicroStack :

```console
busa@busa:~$ sudo microstack init --auto
```
Cette commande configure un déploiement OpenStack à nœud unique avec les configurations par défaut.

![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/872f7453da5854fa39a89fd0758d24f752f63617/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_rj2mb040us8i928q8tid.webp)

Le message d'erreur signifie qu'avec l'option `--auto`, MicroStack nécessite de spécifier un rôle : soit `--compute`, soit `--control`.

Vous devez relancer la commande en précisant un rôle.  
Si c'est le premier et unique nœud (installation à nœud unique), choisissez `--control` :
```console
busa@busa:~$ sudo microstack init --auto --control
```
![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/9d73ba3f09d0cafa87a421bf8bf287ebb830c98a/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_a0yr0wuq7o6i75j4ql4c%20(1).webp)
![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/f8e7a406e977ef54a1768806f7a0350616db98fe/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_lfo8ltdn716r9p0aak74.webp)

Cette commande configure votre VM Ubuntu en tant que nœud unique de contrôle.
Cette installation inclut :
Le tableau de bord OpenStack (Horizon)
Les points de terminaison API (API endpoints)
Les capacités de calcul
Idéal pour tester ou apprendre OpenStack.

Ensuite, exécuter la commande suivante :
```console
busa@busa:~$ microstack.openstack endpoint list
```
But : Affiche tous les points de terminaison des services OpenStack (URLs internes, publiques et admin).

![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/f59154b13ee52cbddd620bc190cf9dffaf30de52/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_k82d8hw3r3bexcwv2i6q.webp)

Cela recherche toutes les entrées liées au tableau de bord ou à Horizon (généralement dans la section des URLs publiques).
## Accéder au tableau de bord OpenStack
Récupérer l'URL du tableau de bord :
```console
busa@busa:~$ sudo microstack.openstack dashboard url
```
J'ai rencontré cette erreur.

![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/dcdcf493ab671f79594173589de02abaff9d8109/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_wuro9yzqlh00ctczoxs7%20(1).webp)

`openstack dashboard url` n'est pas une commande valide du CLI OpenStack.

Voici ce qu'il faut faire à la place (pas besoin du CLI pour obtenir l'URL du tableau de bord) :

### Accéder au tableau de bord Horizon pour MicroStack
1. Récupérer l'adresse IP de votre VM Ubuntu  
   Exécuter dans le terminal Ubuntu :
```console
busa@busa:~$ ip a
```
![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/d8bb5c6d41f8f572e0e76a00f6dbd6fb54c3e369/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_9mxs275hm4hzxnue5vxx.webp)

Vous cherchez cette information : `inet 192.168.129.128`  
Cette IP `192.168.129.128` est celle dont vous avez besoin pour accéder au tableau de bord via le navigateur.

### Accéder au tableau de bord dans un navigateur

Sur votre machine hôte (par exemple Windows ou Mac), ouvrez un navigateur et allez à :  
![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/8e15f2c7e96454cf3aa9782c1b55b58cb827be3d/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_6oymdsna78l8e3cdwk2y.webp)

Vous pourriez voir un avertissement de sécurité. Ce n'est pas un problème — le tableau de bord utilise un certificat auto-signé.

Cliquez sur : **Avancé → Continuer (non sécurisé)**
![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/5414636f99647915310d39a9cc4786fca34987ce/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_rk56hpj4nm9nuqyckrvb.webp)

Obtenir le mot de passe administrateur

Dans votre terminal, exécutez la commande suivante :

```console
busa@busa:~$ sudo snap get microstack config.credentials.password
```
![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/d1093f807b184aa5877eb3cfc30ca94cc31a4a86/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_zbf8qoce121wa4yszf8b.webp)

Cette erreur signifie que le mot de passe n’est pas stocké sous
config.credentials.password.  
Cela peut arriver avec les versions récentes de MicroStack ou si
l’initialisation ne s’est pas complètement terminée.

Essayons plutôt cette méthode pour récupérer les identifiants administrateur :

Exécutez la commande suivante :
```console
busa@busa:~$ sudo cat /var/snap/microstack/common/etc/microstack.rc
```
![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/033056c05cc606ac5df7788ae3974710aecb323e/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_drqv9mshxbizjln1h9dr.webp)

Ce fichier contient souvent les variables d’environnement exportées,
y compris le nom d’utilisateur, le mot de passe, le nom du projet
et le point d’accès (endpoint) OpenStack.

Essayons maintenant d’accéder au tableau de bord Horizon.

![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/4a9692cb409a89142fd626cacea139a70dfca09f/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_n4ten6brfi39sph4o055.webp)

![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/a9191045909d52462e041dc5ec3b57f1fecf7e02/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_uv0iiffz5lwwpx1dsdn1.webp)

Connexion effectuée avec :
- Nom d’utilisateur : admin
- Mot de passe : récupéré depuis le fichier microstack.rc

Astuce (pour faciliter le chargement des identifiants dans le terminal) :
Pour utiliser l’OpenStack CLI plus tard, vous pouvez charger les identifiants avec la commande :
source /var/snap/microstack/common/etc/microstack.rc

Objectif :
Mettre en place un cloud privé à l’aide de MicroStack, lancer une instance,
et documenter l’accès via une Floating IP et le tableau de bord Horizon.

## Étape 1 : Créer une paire de clés (Key Pair) pour l’accès SSH aux instances
Chemin :
Project > Compute > Key Pairs

![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/98e23165f818787fb1e944ce7816b3fe11b943f6/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_9gcxryky46ezr0o0slye.webp)

Cliquez sur **Create Key Pair**  
Donnez-lui un nom, par exemple : `busa-key`

![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/42eb067d19e8f453c745857dc02066d1c63a839a/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_shcfaomt305umij6e7v7.webp)

Enregistrez la clé privée (fichier `.pem`) sur votre machine hôte
![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/6ee00a90f7bcc4b80279eabb95c0852241ac7f1f/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_oz8ug0gedl0fltzxxb70.webp)

## Étape 2 : Télécharger une image

Allez à : Project > Compute > Images  

1. Cliquez sur **Create Image**

![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/847b0c8387466d4fbefa2a01febbcc0c4daa1edb/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_0cgiiq7smwmgwmukzojp.webp)

Remplissez les champs :

- **Nom :** ubuntu-22.04  
- **Source de l'image :** URL ou fichier  
- **Format :** QCOW2  

Vous pouvez utiliser l'image Ubuntu Cloud :  
[https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img](https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img)

Ouvrez un navigateur dans la VM Ubuntu, copiez et collez le lien ci-dessus et téléchargez l'image Ubuntu.  

Alternativement, vous pouvez télécharger l'image Ubuntu Cloud directement depuis le terminal de la VM Ubuntu :  
```console
busa@busa:~$ wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img -O ubuntu-22.04.img
```
![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/e8c61f68a667b71ee3dc51f9a687622e4913bdcd/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_s1x5ftu3hc9zb3rkp3wp.webp)

Télécharger l'image sur OpenStack (MicroStack)

Maintenant, téléchargez l'image en utilisant le CLI `microstack.openstack` :
![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/de0b24b4c82a6d3c17c8b47fac8985bef2e6d58f/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_uztwigd92q8p2s0xbi11.webp)

Mon image **Ubuntu-22.04** a été créée et enregistrée avec succès dans OpenStack.  
Le statut est **active**, ce qui signifie qu'elle est prête à être utilisée.

Vérifions si elle est bien listée :  

Exécutez la commande suivante :

```console
busa@busa:~$ microstack.openstack image list
```
![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/3745b50105a9da18fb779cdc28119f4a05a684e2/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_p5h7hrs2x9d3mscmu4ld.webp)

Oui, elle est bien listée comme montré ci-dessous dans l'image.

![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/000138cfac30715ddc06313f43937569b8663638/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_dfcys912l3hsw83fak5h.webp)

Étape suivante : Lancer une instance

Vous pouvez lancer l'instance depuis le tableau de bord Horizon :

Remplissez le formulaire **Launch Instance** :

- **Nom de l'instance :** my-first-vm  
- **Nombre d'instances :** 1  
- **Availability Zone :** nova  
- **Source de démarrage :** Choisir une image  
- **Nom de l'image :** Ubuntu-22.04  
- **Flavor :** m1.small  
- **Réseau :** sélectionner généralement `external`  
- **Key Pair :** sélectionner pour accéder à la VM via SSH  

Cliquez sur **Launch Instance** pour démarrer l'instance.

---

Ou utilisez le terminal pour obtenir le même résultat :

```console
busa@busa:~$ microstack.openstack server create \
--flavor m1.small \
--image "Ubuntu-22.04" \
--network external \
--key-name microstack \
my-first-instance
```
![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/d30296e6419c0eb15da3f164cd22c2645cda32a5/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_62y1irhm80sjbbfz1zux.webp)

J'ai rencontré une erreur car je n'avais pas ajouté ma paire de clés `busa-key.pem`.  
Voici la commande correcte :


```console
busa@busa:~$microstack.openstack server create \
--flavor m1.small \
--image "Ubuntu-22.04" \
--network external \
--key-name busa-key \
my-first-instance
```

![image alt](https://github.com/NovaMou/OpenStack-Lab/blob/c5b45cb255a421a4a5411f348d883a917d08b872/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_dy0mv5dj2456vptbcxme.webp)


## Résumé du projet : Déploiement de VM MicroStack/OpenStack sur Ubuntu

### Objectif
Installer et configurer OpenStack (MicroStack) sur Ubuntu avec Snap,  
et lancer avec succès une instance de machine virtuelle (VM).

### Problèmes rencontrés


- **Problème :** Connexion réseau échouée  
  **Cause :** DHCP et DNS manquants  
  **Solution :** Modification de netplan, redémarrage des services, ajout de `resolv.conf`

- **Problème :** Erreurs lors de l'installation Snap  
  **Cause :** Problème de résolution de noms  
  **Solution :** Résolu après correction du DNS

### Ce que j'ai appris
Ce projet m'a permis de :

- Acquérir des connaissances pratiques sur le réseau Linux et Netplan  
- Déboguer des problèmes de connectivité dans les VM  
- Installer OpenStack via Snap avec MicroStack 
