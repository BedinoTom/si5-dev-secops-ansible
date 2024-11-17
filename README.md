# TP3 - ANsible - DevSecOps

Dans le cadre de notre TP3, nous avons déployé une instance EC2 afin d’exécuter notre programme via CircleCI et Docker. Pour garantir un environnement reproductible, notamment en cas de défaillance de la machine virtuelle (désignée par la suite comme VM) ou d'erreurs majeures de configuration, nous avons cherché à simplifier le processus de remise en état.

Pour cela, nous avons utilisé Ansible, un outil d'automatisation qui nous permet d'assurer une configuration identique entre l'environnement de préproduction (preprod) et l’instance EC2. Grâce à des playbooks définissant précisément les tâches à effectuer, Ansible garantit une cohérence totale des environnements, facilitant ainsi le déploiement et la gestion de l'infrastructure.

## Organisation

Voici l'organisation des fichiers :

```
Racine
├── roles
│    ├── add-key
│    ├── fail2ban
│    ├── firewall
│    ├── install-docker
│    └── sshd
│         └── templates
│                └── sshd_config.txt.j2
├── ansible.cfg
├── inventory.ini
└── playbook.yml
```
- **roles/** : Dossier contenant les rôles Ansible, chacun dédié à une tâche ou un ensemble de configurations spécifiques.
- **roles/add-key/** : Rôle permettant d'ajouter une clé SSH pour l'accès sécurisé aux machines.
- **roles/fail2ban/** : Rôle configurant Fail2Ban, un outil de prévention contre les attaques par force brute.
- **roles/firewall/** : Rôle responsable de la configuration du pare-feu (comme UFW ou iptables) pour sécuriser l'accès réseau.
- **roles/install-docker/** : Rôle installant Docker sur les machines cibles, afin de faciliter le déploiement de conteneurs.
- **roles/sshd/** : Rôle dédié à la configuration de sshd, le démon SSH, pour sécuriser et personnaliser l'accès au serveur.
- **roles/sshd/templates/sshd_config.txt.j2** : Modèle Jinja2 pour le fichier de configuration SSHD (sshd_config). Utilisé pour personnaliser dynamiquement la configuration selon les variables définies.

- **ansible.cfg** : Fichier de configuration global d'Ansible. Il définit des paramètres importants comme l'inventaire à utiliser, le répertoire des rôles, ou les options de connexion SSH.
- **inventory.ini** : Fichier d'inventaire listant les machines cibles et leurs groupes. Utilisé pour organiser les serveurs et spécifier les détails de connexion.
- **playbook.yml** : Playbook principal d'Ansible. Il orchestre les rôles et définit les étapes nécessaires pour configurer les machines cibles.