# 1. Ansible

- Provisioning and configuration management:
  - Of our infrastructure
  - In a automated fashion
  - Using files -- YAML files
  -
- Simple yet powerful tool
  - No additional software to be installed on the server.
  - Uses ssh protocol for connecting and configuring the server.
  - Just needs Python
- It's alternatives: Puppet and Chef.
- take care of a lot of servers
- Ansible use cases:
  - Installing our entire LAMP stack on new server
  - Configuration of most of the software on our servers
  - Testing if servers are configured properly
  - Deploying our applications
  - Infrastructure As Code (IAC) is in a single Git repository

# 2. Up and run Vagrant

- [Install Vagrant](https://www.vagrantup.com/downloads)
- Install VirtualBox if you face this error:

  ```cmd
  No usable default provider could be found for your system.

  Vagrant relies on interactions with 3rd party systems, known as
  "providers", to provide Vagrant with resources to run development
  environments. Examples are VirtualBox, VMware, Hyper-V.

  The easiest solution to this message is to install VirtualBox, which
  is available for free on all major platforms.

  If you believe you already have a provider available, make sure it
  is properly installed and configured. You can see more details about
  why a particular provider isn't working by forcing usage with
  `vagrant up --provider=PROVIDER`, which should give you a more specific
  error message for that particular provider.
  ```

  - `sudo apt update && sudo apt install virtualbox`

- `vagrant up`

# 3. Inventory file

- Before we can actually start using Ansible
- Specify connection to the servers for Ansible.
- Alternative to SSH command

`server_ip_or_domain.com ansible_user=username ansible_private_key_file=my_private_key_file_path`

- Create a file & name it `hosts`
  ```
  [ansible_tutorial]
  192.168.60.70 ansible_user=vagrant ansible_private_key_file="./.vagrant/machines/default/virtualbox/private_key"
  ```
  - `[ansible_tutorial]` is a group name, It's good practice to specify a group, even if you have single server. It's more descriptive way than using IP or host name.

# 4. First experience with ansible

- `ansible all -m ping -i hosts`
  - `all` is the group from inventory file. Though to be more precise I should say all is a keyword which means to execute ansible against all defined servers in the `hosts` file
  - ping module to check the connection.

# 5. Create playbook

- `tasks` is like a single command in bash
- `setup` is internal task for gathering information about the server. We will use them later on. For now, just know, that such additional task exists.

# 6. Roles in Ansible

- Use roles instead of using tasks directly in playbooks
- E.X. In order to install nginx. We can group them into single role. Roles will help you organize our structure.
- Single responsibility -- e.x. installing and configuring nginx.
- Reuseable
- `mkdir roles`
  - `mkdir roles/nginx`
    - Now inside this one you can create other predefined directories:
      - E.X: `tasks`, `defaults`, `handlers`
- `my-touch roles/nginx/tasks/main.yml`. [Read more about my-touch](https://github.com/kasir-barati/the-pragmatic-programmer/blob/main/customize-your-dev-env/my-touch.md)

# 7. Tasks in Ansible

- Each task should start with name.
- Next line contains module name that we will use.
- `ansible-playbook -i hosts playbook.yml`
