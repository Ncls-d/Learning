# Installation d'Ansible
## Installation via APT

```bash
apt update && apt full-upgrade -y
apt install sudo ansible
adduser ansible
usermod -aG sudo ansible
ansible --version
```

## Installation via environnement Python

Un environnement Python est un espace isolé dans lequel on peut installer des paquets Python sans interférer avec les autres projets ou avec l’installation Python globale du système. Il permet d'avoir une bulle autonome, propre et facile à supprimer si besoin.

```bash
sudo apt install -y python3 python3-pip python3-venv git sshpass
python3 -m venv ~/.venv/env-ansible # Création de l'environnement
source ~/.venv/env-ansible/bin/activate # Activation de l'environnement
pip install --upgrade pip
pip install ansible
ansible --version
```

### Fonctionnement de l'environnement Python

```bash
python3 -m venv env-ansible
source env-ansible/bin/activate
deactivate # Sortir de l'environnement
```

### Alias dans .bashrc

```bash
echo "alias env-ansible='source ~/.venv/env-ansible/bin/activate'" >> ~/.bashrc
reboot
```

# Configurer la connexion SSH 
## Avec ssh-copy-id

S'assurer que sshd_config est correctement renseigné :

PubkeyAuthentication yes

AuthorizedKeysFile      .ssh/authorized_keys .ssh/authorized_keys2

#PasswordAuthentication no

```bash
sudo adduser ansible (client)
sudo usermod -aG sudo ansible
ssh-keygen -t rsa -b 4096 (serveur)
sudo ssh-copy-id ansible@IP_CLIENT
```

## Avec copie manuelle

```bash
cat ~/.ssh/id_rsa.pub 
copier la clé
```
sur le client : 
```bash
mkdir -p /home/ansible/.ssh
chmod 600 /home/ansible/.ssh
chown ansible:ansible /home/ansible/.ssh
coller la clé dans authorized_keys
```

## Test de connexion

```bash
ssh ansible@IP_CLIENT
```

# Créer un fichier d'inventaire pour Ansible
## Création de l'inventaire
### Ajout du serveur Ansible

```bash
mkdir ~/ansible
cd ~/ansible
nano hosts  # ou hosts.ini, etc.
```
Ajouter les lignes suivantes :
```bash
localhost ansible_connection=local
```

### Ajout du serveur distant Debian 12

```bash
[<nom_du_groupe>]
<alias_du_serveur_distant> ansible_host=<adresse_ip_du_serveur_distant>
ansible_user=<utilisateur_sur_le_serveur_distant>
exemple :
[debian]
debian-ansible ansible_host=IP_CLIENT ansible_user=ansible
```

## Test du bon fonctionnement d'Ansible

```bash
ansible all -m ping
```
Si le chemin vers l'inventaire n'est pas par défaut :
```bash
ansible all -m ping -i /chemin/vers/inventaire
```

# Créer et lancer son premier playbook Ansible
## Ecriture d'un playbook minimal

```bash
mkdir ~/ansible/projet-1/
nano setup.yaml
```

Attention c'est un .yaml, l'indentation est donc extrêmement importante : 

```yaml
- name: Installation de base et création d'utilisateur
  hosts: debian-ansible
  remote_user: ansible
  become: yes
  become_method: sudo

  tasks:
    - name: Installer le paquet htop
      ansible.builtin.apt:
        name: htop
        state: present

    - name: Creer l'utilisateur tutotech_admin
      ansible.builtin.user:
        name: tutotech_admin
        shell: /bin/bash
        create_home: yes

    - name: Autoriser la clé SSH pour tutotech_admin
      ansible.posix.authorized_key:
        user: tutotech_admin
        state: present
        manage_dir: yes
        key: "{{ lookup('file', 'files/id_rsa.pub') }}"
```

Pour que la tâche Autoriser la clé SSH pour tutotech_admin fonctionne il faut au préalable créer un
dossier files et déposer dans ce dossier une clé publique nommée id_rsa.pub :

```bash
cd ansible/projet-1/
mkdir files
cp /root/.ssh/id_rsa.pub /root/ansible/projet-1/files/id_rsa.pub
```

## Exécution du playbook

```bash
ansible-playbook -i ~/ansible/hosts setup.yaml
```

Il est probable que l'erreur suivante apparaisse : 

<p align="center">
    <img src="Fail_playbook.png" alt="Fail_playbook" style="width: 1000px;" />
</p>

Cela signifie que le mot de passe sudo n'a pas été renseigné. 

L’option --ask-become-pass permet d’entrer le mot de passe sudo de l’utilisateur qui va exécuter les
commandes sur le serveur Debian 12 :

```bash
ansible-playbook -i ~/ansible/hosts setup.yaml --ask-become-pass
```

<p align="center">
    <img src="success_playbook.png" alt="success_playbook" style="width: 1000px;" />
</p>

La première exécution indique toutes les tâches en "changed". 

Si on veut qu'à l'avenir Ansible exécute les commandes sudo sans demander de mot de passe, aller sur le serveur distant :

```bash
echo 'ansible ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/ansible
chmod 440 /etc/sudoers.d/ansible
```

Si on relance le playbook, l'indempotence d'Ansible fait que tout sera OK : 

<p align="center">
    <img src="success_playbook2.png" alt="success_playbook2" style="width: 1000px;" />
</p>

## Vérifications sur la machine distante

```bash
htop
cat /etc/passwd|grep tutotech_admin
sudo cat /home/tutotech_admin/.ssh/authorized_keys
```

# Ecrire un playbook Ansible simple

Le but ici est d'utiliser un playbook Ansible pour :
  - installer le logiciel cmatrix
  - créer un utilisateur tutotech_stagiaire
  
```bash
- name: Installation cmatrix et création user
  hosts: debian-ansible
  become: yes
  gather_facts: yes

  tasks:
    - name: Installer cmatrix via apt
      ansible.builtin.apt:
        name: cmatrix
        state: present
        update_cache: yes
      tags: install_cmatrix

    - name: Vérifier que cmatrix est installé
      ansible.builtin.command: which cmatrix
      register: cmatrix_check
      changed_when: false
      tags: verify_cmatrix

    - name: Afficher le résultat de la vérification cmatrix
      ansible.builtin.debug:
        msg: "cmatrix est installé dans {{ cmatrix_check.stdout }}"
      when: cmatrix_check.rc == 0

    - name: Créer le user tutotech_stagiaire
      ansible.builtin.user:
        name: tutotech_stagiaire
        shell: /bin/bash
        create_home: yes
        comment: "Utilisateur pour le TP Ansible"
      tags: create_user

    - name: Vérifier que le user existe
      ansible.builtin.command: "id tutotech_stagiaire"
      register: user_check
      changed_when: false
      ignore_errors: yes
      tags: verify_user

    - name: Afficher le résultat de la vérification utilisateur
      ansible.builtin.debug:
        msg: "Le user tutotech_stagiaire existe (UID: {{ user_check.stdout | regex_search('uid=(\\d+)') | default('N/A') }})"
      when: user_check.rc == 0
```

# Utiliser les rôles Ansible dans un playbook
## Contexte et préparation de la structure du rôle
### Création du répertoire et copie de la clé SSH

```bash
mkdir -p ~/ansible/projet-2/roles
cd ~/ansible/projet-2
ansible-galaxy init roles/users

cp ~/ansible/projet-1/files/id_rsa.pub ~/ansible/projet-2/roles/users/files/
```

### Configuration du rôle (roles/users/tasks/main.yml)

```bash
- name: Créer l'utilisateur tutotech_technicien
  ansible.builtin.user:
    name: tutotech_technicien
    shell: /bin/bash
    create_home: yes
    comment: "Utilisateur technique pour accès SSH"

- name: Déployer la clé SSH
  ansible.posix.authorized_key:
    user: tutotech_technicien
    state: present
    key: "{{ lookup('file', 'id_rsa.pub') }}"
```

### Playbook principal (site.yml)

```bash
- name: Déploiement de l'utilisateur technique
  hosts: SRV-DEB12
  become: yes
  roles:
    - users
```

### Exécution et vérification

```bash
cd ~/ansible/projet-2
ansible-playbook -i ~/ansible/hosts site.yml
```

Sur la machine distante
```bash
id tutotech_technicien

sudo -u tutotech_technicien cat /home/tutotech_technicien/.ssh/authorized_keys
```