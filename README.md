
<div class="title-block" style="text-align: center;" align="center">

Astra-One Cloud - Ansible

<p><img title="logo" src="assets/logo.png" width="320" height="320"></p>

</div>
## Introduction

> [!WARNING]  
> La solution suivante est en développement et n'est pas totalement au point.

Astra-One Cloud est une plateforme de cloud privé sécurisé conçue pour héberger, exploiter et administrer des services internes critiques au sein d’une infrastructure totalement maîtrisée. Contrairement à une architecture distribuée dans un cloud public, Astra-One Cloud repose sur un modèle souverain : les ressources physiques, les mécanismes de virtualisation, le système d’exploitation, la segmentation réseau et la gestion des secrets sont intégralement contrôlés en interne.

La plateforme s’appuie sur une architecture virtualisée construite autour d’un hyperviseur centralisé, permettant la création de machines virtuelles isolées. Chaque composant est intégré dans une logique de cloisonnement et de séparation des responsabilités. Les secrets ne sont jamais stockés en clair et sont gérés par HashiCorp Vault. Le système d’exploitation standardisé est Rocky Linux afin d’assurer stabilité, cohérence et support à long terme.

Astra-One Cloud ne doit pas être compris comme un simple cluster de machines virtuelles. Il s’agit d’une architecture structurée selon des principes de “Security by Design”, où la sécurité est une propriété intrinsèque et non une surcouche ajoutée.  

---
## Architecture

> [!IMPORTANT]  
> Par ailleurs, toutes les VM utilisées sont sous Rocky Linux 9, RHEL 10 n’étant pas encore supporté par OpenNebula.

![preview](assets/preview.png)
Astra-One Cloud repose sur une architecture modulaire :

- **Frontend Node**
    - OpenNebula Core
    - Sunstone GUI
    - Base de données SQL
        
- **Hypervisor Nodes**
    - KVM    
    - Virtualisation des VM
        
- **Storage Layer**
    - Stockage SAN
        
- **Bastion**
    - Solution Vault par HashiCorp
## Ansible dans Astra-One Cloud

Ansible ne sert pas juste à “installer des paquets”.  
Dans le cadre d’Astra-One Cloud, il va permettre de transformer l’architecture en **infrastructure reproductible, cohérente et sécurisée** tout en **réduisant les erreurs humaines** dans le processus.
Dans un premiers temps, Ansible va permettre une **mise en place simplifiée de l’infrastructure**.
En effet, au lieu d'installer et de paramétrer les nodes à la main, Ansible va :
- Déployer automatiquement les rôles (frontend, hypervisor, vault, storage)
- Installer les dépendances
- Appliquer une configuration standardisée
- Garantir que chaque nœud respecte les mêmes configurations de sécurité
    
 Résultat : un déploiement reproductible en quelques commandes.
De ce fait, ça aussi **faciliter l’augmentation du nombre de nœuds back-end**.
Pour ajouter un nouvel hyperviseur, il suffit d'ajouter une ligne dans l’inventory , et de lancer le playbook pour que **le serveur soit configuré automatiquement**.
Cela permet :
- Une meilleur scalabilité de la structure
- D’ajouter de la capacité sans reconfiguration manuelle
- De maintenir la cohérence de sécurité entre tous les nœud
- Facilite l'administration de tout les nodes
    
## Configurations

```
astra_ansible_configuration/
│
├── README.md
│
├── ansible.cfg
│
├── front/
│    └── install_front.yml
│
└── inventory/
     └── inventory.yml

```
