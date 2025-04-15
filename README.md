Voici un **cours d’introduction à Ansible pour l’automatisation des réseaux**, structuré de manière claire et progressive. Ce cours est idéal pour les administrateurs réseau, ingénieurs systèmes, ou toute personne souhaitant automatiser la configuration réseau avec Ansible.

---

## 📘 **Cours : Automatisation des Réseaux avec Ansible**

### 🎯 Objectifs :
- Comprendre le fonctionnement d’Ansible
- Automatiser la configuration d’équipements réseau (Cisco, Juniper, etc.)
- Gérer des tâches réseau à grande échelle
- Réaliser des playbooks efficaces pour le réseau

---

## 1. 🔧 Introduction à Ansible

### Qu’est-ce qu’Ansible ?
- Un outil d'automatisation **open source**
- Agentless : utilise SSH (Linux) ou WinRM (Windows)
- Fichiers de configuration en YAML (faciles à lire et écrire)

### Avantages pour les réseaux :
- Configuration centralisée
- Automatisation rapide sans agents
- Compatible avec plusieurs plateformes réseau (Cisco, Juniper, Arista, etc.)

---

## 2. 🛠️ Installation d’Ansible

### Sur une machine Linux (Ubuntu/Debian) :
```bash
sudo apt update
sudo apt install ansible
```

### Sur RHEL/CentOS :
```bash
sudo yum install epel-release
sudo yum install ansible
```

---

## 3. 📂 Structure de base d’un projet Ansible

```
network-automation/
├── inventory/
│   └── routers.yml
├── playbooks/
│   └── config_interfaces.yml
├── group_vars/
├── roles/
└── ansible.cfg
```

---

## 4. 🧭 Inventaire pour les équipements réseau

### Exemple d’inventaire `routers.yml`
```yaml
all:
  children:
    cisco_routers:
      hosts:
        R1:
          ansible_host: 192.168.1.1
          ansible_user: admin
          ansible_password: cisco
          ansible_network_os: ios
```

---

## 5. ✍️ Écrire un Playbook pour les équipements réseau

### Exemple : configuration d’une interface sur Cisco IOS
```yaml
- name: Configurer les interfaces Cisco
  hosts: cisco_routers
  gather_facts: no
  connection: network_cli

  tasks:
    - name: Configurer l’interface GigabitEthernet0/1
      ios_config:
        lines:
          - description Connexion WAN
          - ip address 10.0.0.1 255.255.255.0
        parents: interface GigabitEthernet0/1
```

---

## 6. 📦 Modules Réseau courants

| Module          | Description                          |
|----------------|--------------------------------------|
| `ios_config`    | Configurer les équipements Cisco IOS |
| `ios_facts`     | Récupérer des informations système   |
| `ios_command`   | Exécuter des commandes CLI           |
| `nxos_config`   | Cisco Nexus                          |
| `junos_config`  | Juniper JUNOS                        |
| `eos_config`    | Arista EOS                           |

---

## 7. 🔁 Boucles et conditions dans les playbooks

### Exemple avec une boucle pour plusieurs interfaces :
```yaml
- name: Configurer plusieurs interfaces
  ios_config:
    lines:
      - no shutdown
    parents: "interface {{ item }}"
  loop:
    - GigabitEthernet0/1
    - GigabitEthernet0/2
```

---

## 8. 🔐 Sécurité : gestion des mots de passe avec Ansible Vault

### Créer un fichier chiffré :
```bash
ansible-vault create secrets.yml
```

### Utiliser dans un playbook :
```yaml
vars_files:
  - secrets.yml
```

---

## 9. 🔍 Debug et test

- Utilisez `ansible-playbook -C` pour simuler les changements (check mode)
- Utilisez `ansible-playbook -vvv` pour le mode verbeux
- Utilisez `show run` ou `show ip interface brief` avec `ios_command` pour la vérification

---

## 10. 📚 Bonnes pratiques

- Utilisez des **rôles** pour organiser vos tâches
- Évitez les commandes brutes, utilisez les **modules spécifiques**
- Testez d’abord en **sandbox**
- Mettez en place un **Git** pour versionner vos configurations

---

## 📘 Ressources complémentaires

- [Documentation officielle Ansible](https://docs.ansible.com/)
- [Ansible for Network Automation (Cisco)](https://developer.cisco.com/ansible/)
- Livres : *Network Programmability and Automation* de Jason Edelman
