---
slideOptions:
  transition: concave
  theme: moon
---
# DAT Brief6

---


### Besoins

- Mettre en place une application de vote supporté par une base de donnée redis.

---

### Comment ça marche
- L'application stocke les données de votes sur une base de donnée. L'utilisateur a seulement accés au site et seule l'application a accés à la base de donnée.


---

### Contraintes
- L'application doit être accessible même si la charge est trop grande sur le cpu 
- la base de donnée doit pouvoir stocker des données sans arriver à saturation et les rendre toujours disponible pour l'app.


---

### Choix d'architecture
- architecture conteneurisé : 
    - application n'a pas une utilisation de ressources constante, ponctuelle ce qui va permettre d'utiliser une config minimum pour la maintenir mais ne pas hésiter à scale out si la charge cpu devenait trop grande. 
    - la bdd va elle aussi être déployée sur un conteneur lié à un stockage persistant qui conservera les données même si la bdd est supprimé.

---

### Ressources à prévoir

- Cluster Azure munis de plusieurs nodes
- Cluster composé de 4 nodes
- régle de scaling
- secrets
- stockage persistant
- ingress

---

### Côté Réseau

<img src="https://i.imgur.com/auqOfg4.png" alt="drawing" width="500"/>


---

### Côté Node

<img src="https://i.imgur.com/EzfIILk.png" alt="drawing" width="700"/>

