# DAT

## Objectif 

L’application Voting App doit être accessible aux utilisateurs via internet cette
dernière doit être mise à jour automatiquement.

## Solution

Utilisation d’un script K8S afin de déployer une première fois l’infrastructure.
Puis mise en place d’un pipeline GitHub Action qui modifie la Voting App pour
la mettre à jour.

## Pourquoi GitHub ACTION ?

Tout d’abord c’est un service en ligne (ne nécessite pas de déploiement en
amont) de plus notre code est déjà sur GitHub et finalement c’est un outil
moderne et en plein essors.


### Explication du pipeline

Avant l'éxécution du pipeline, un cluster doit être déployé sur Azure, et l'infrastructure de base doit y être déployée. 

Une fois déployée, l'infrastructure pourra être mise à jour via le pipeline suivant :

```mermaid
flowchart LR
    first_start[Premier déploiement] --> kubernetes[Modèle Kubernetes]

    subgraph pipeline[Github Action]
        cron[Cron - toutes les heures] --> api[Récupération version via DockerHub]
    end
    
    api -. Mets à jour l'image .-> kubernetes
    
    kubernetes --> Azure
 ```


Grâce à un "cron", le pipeline s'éxécute à heures fixes (toutes les heures), lors de son éxécution, un requête à l'API de Docker Hub est faite afin de récupérer le numéro de version de l'image à déployer. Ce numéro de version est ensuite inscrit de le fichier "Modèle Kubernetes" qui est ensuite utilisé pour appliqué cette modification sur Azure.


### Pricing

Via [l'outil de calcul des tarifs](https://azure.microsoft.com/fr-fr/pricing/calculator/) fournit par Microsoft : 
![](https://i.imgur.com/ZVOIuFV.png)


Le choix de la catégorie de machine virtuelle, fait à l'aide de la [documentation d'Azure](https://azure.microsoft.com/fr-fr/pricing/details/virtual-machines/series/), s'est porté vers la série B, en effet c'est la série la plus adaptée pour de petites charges, c'est également la plus économique. Il existe aussi la série A mais elle reste cependant plus adaptée aux tests et au développement. 
De même pour le stockage, nous avons choisi un SSD E1 car se sont les moins chers pour l'espace de stockage dont nous avons réellement besoin, ils sont également plus rapides que des HDD.
