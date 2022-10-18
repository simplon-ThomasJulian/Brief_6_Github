---
slideOptions:
  transition: slide
  theme: moon
---

# Executive Summary Brief 6

---

## Déploiement cluster AKS
- az cli déploiement cluster AKS: ```az aks create -g ressourcegroup -n clusteraks --ssh-key-value ./.ssh/id_rsa.pub --node-count```
- liaison cluster/kubectl : ```az aks get-credentials -n clusteraks -g ressourcegrou```

---

## Container Redis
- création manifest kubernetes: déploiement container faisant tourner redis grace```kubectl apply -f```
- service loadbalancer pour communiquer
- vérifier son état grace au client redis

---

## Container Voting App
-  même chose que pour redis avec des variables d'environnements supplémentaires, l'image de l'app et un service de type Loadbalancer
-  changer le type du loadbalancer de redis vers clusterip.
-  L'app est accessible sur le web en http.

---

## Password Redis
- création manifest de type secret.
- On écrit le manifest pour rendre facile et compréhensible l'ajout de la variable lié au mot de passe au manifest redis et votingapp.

---

## Stockage Redis
- Création manifest permettant le déploiement d'un storage class
- Standard_LRS en SKU pour conserver les données.
- un autre manifest persistant volume claim permet d'allouer une partie du stockage déployé à la bdd.

---

- inclusion d'une nouvelle spécification dans le manifest de redis pour lui allouer ce stockage et monter les données vers ce point.
- suppression et remise en place de la bdd redis confirme que les données sont bien stockés à part.

---

## Application gateway avec AKS
- création nouveau cluster comprenant l'add on AGIC et l'app gateway : ```az aks create -g ressourcegroup -n clusteraks --ssh-key-value ./.ssh/id_rsa.pub --node-count 4 enable-managed-identity -a ingress-appgw --appgw-name Applicationgateway --appgw-subnet-cidr "10.225.0.0/16```
- Je crée un manifest créant une ressource ingress qui route les arrivées sur l'ip publique de la gateway vers l'app.

---

## Nom de domaine
- Je relie un nom de sous-domaine par gandi.net avec l'adresse ip public de la gateway

---

## Certificat TLS pour App
- je crée un certificat avec le client certbot de gandi : ```sudo certbot certonly --authenticator dns-gandi --dns-gandi-credentials .../gandi.ini -d domain.name```
- j'importe le certificat via un secret kubernetes que j'inclus dans le manifest d'ingress pour qu'il soit implémenté à chaque déploiement.

---

## Scaling
- modification de la variable replicas à 2, création manifest de type HorizontalPodAutoscaler configuré avec les règles imposées en ajoutant des specs régulant l'utilisation du cpu par l'app.
- Je réutilise le script du brief 4 et on constate le scale out des pods et leur scale in une fois la charge passée.

---

# Fonctionnement de Kubernetes
- Système de gestion de conteneurs
- automatise les processus
- cluster k8S --> ensemble de nodes

---

- control plan et data plan
- nodes gèrent des pods déployant des conteneurs en leur sein
- fonctionne sur la base d'un état défini et un réel
- contrôleurs gèrent l'état du cluster pour qu'il corresponde à l'état souhaité


# Difficultés rencontrées
- Syntaxe différente pour les manifest kubernetes
- appréhender le fonctionnement des containers et leurs liaisons entre nodes/pods
- "ingress"

---

### Lien vers le Github

https://github.com/simplon-ThomasJulian/Brief_6_Github

---