# Ansible Lab 2

This repository contains an Ansible playbook and roles for setting up a web server, Jenkins, and MariaDB on different hosts. Below is an overview of the structure and usage.

## Inventory
The `inventory` file defines the hosts and their groups:

```ini
[server]
server.example.com ansible_host=34.235.153.245

[jenkins]
jenkins.example.com ansible_host=54.89.10.29

[db]
db.example.com ansible_host=192.168.1.100
```

## Playbook
The `site.yaml` playbook orchestrates the setup of the web server, Jenkins, and MariaDB:

```yaml
---
- name: install nginx and add custom web page
  hosts: server
  become: true
  roles:
    - roles/webserver

- name: Install and Run Jenkins
  hosts: jenkins
  become: true
  roles:
    - roles/jenkins

- name: Install and Run MariaDB
  hosts: db
  become: true
  vars_files:
    - roles/mariadb/vars/main.yml
  roles:
    - roles/mariadb
```

## Roles
### Webserver
- Installs and configures Nginx.
- Deploys a custom web page using a Jinja2 template.

### Jenkins
- Installs Java and Jenkins.
- Starts the Jenkins service.
- Retrieves the initial admin password securely.

### MariaDB
- Installs MariaDB server.
- Configures the root password.
- Copies a custom `my.cnf` configuration file.

## Usage
1. Clone the repository:
   ```bash
   git clone https://github.com/<your-username>/ansible-lab2.git
   cd ansible-lab2
   ```

2. Run the playbook:
   ```bash
   ansible-playbook site.yaml --ask-vault-pass
   ```

3. Verify the setup:
   - Access the web server at `http://server.example.com`.
   - Access Jenkins at `http://jenkins.example.com:8080`.
   - Connect to MariaDB on `db.example.com`.

## Requirements
- Ansible 2.9+
- RHEL-based hosts
- Python 3 with `PyMySQL` installed on the database host.

## License
This project is licensed under the MIT License.