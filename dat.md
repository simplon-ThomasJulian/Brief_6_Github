# DAT Brief6

graph TB
        ipp-.->util{Utilisateur}
        node1--->pod1 & pod2
        pod2 ---> dd
    subgraph az[Azure]
        appgw{application gateway}--->lb[loadbalancer]
        ipp[IP Publique]<---appgw
        lb-.->node1 & node2 & node3 & node4
        dd[Stockage Persistant]
        subgraph kube[Kubernetes]
        node1[Node 1]
        node2[Node 2]
        node3[Node 3]
        node4[Node 4]
        end
        subgraph pods[Node 1]
        cluster[Cluster IP Vote]
        cluster ---> pod1
        ing[Ingress]
        cluster ---> ing
        pod1[Application de Vote]
        pod2[Base de donnée Redis]

        end        
        subgraph cont[Container]
        voteapp[Application de Voting]
        end
        ing---appgw
end


---
slideOptions:
  transition: slide
  theme: moon
---
# DAT Brief6

---


### Besoins

- Mettre en place une application de vote supporté par une base de donnée redis.

---

### Comment ça marche
- L'application stocke les votes sur une base de donnée. L'utilisateur a seulement accés au site et seule l'application a accés à la base de donnée.


---

### Contraintes
- L'application doit toujours être accessible même si la charge est trop grande sur le cpu, la base de donnée doit pouvoir stocker des données sans arriver à saturation et les rendre toujours disponible pour l'app.


---

### Choix d'architecture
- architecture conteneurisé car l'application n'a pas une utilisation de ressources constante mais ponctuels ce qui va permettre d'utiliser une config minimum pour la maintenir mais ne pas hésiter à scale out si la charge cpu devenait trop grande. 
- Par facilité la bdd va elle aussi être déployé sur un conteneur avec un stockage persistent qui conservera les données même si la bdd est supprimé.

---

<img src="https://i.imgur.com/nXufFuh.png" alt="drawing" width="700"/>


---

<img src="https://i.imgur.com/EzfIILk.png" alt="drawing" width="700"/>

---