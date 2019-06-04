## Intro

These are Ansible scripts used for vm orchestration and configuration management.

# Installation

1. Please install [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
2. Run `sudo pip install -U pywinrm` to allow Ansible to communicate with Windows nodes.
3. Install [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) and run `azure login` to setup azure cli.

# vm_create_windows2016.yml

This script creates a Windows Server 2016 VM in Azure and configures winrm to be "ansible friendly", meaning a certificate will be created and **winrm-https** will be open. Along Windows Server, other necessary resources--such as a resource group--will be created. This is **idempotent code**, which means it will not re-create a resource or vm if it already exists. So you can add this to an already existing resource group.

### Instructions

1. Edit **vm_create_windows2016.yml** variables to your liking.
2. Run `ansible-playbook ./vm_create_windows2016.yml`

### Idiosyncrasies

1. **azure_rm_storage_accoun**t: name must be entirely uniqe in azure and can only consist of numbers and letters, and only between 3-24 characters long. Therefore, '{{ resource_group }}{{ storage_acct_append }}' is used. Please keep in mind to make these names short and only consist of alpha-numeric characters.

### To test Ansible connection

1. Go to [portal.azure.com](portal.azure.com/) and click on the new vm. Copy the ip address.

1. On your local machine, edit the /etc/ansible/host (require elevated privileges). Add the ip address to a new line anywhere in the file. e.g. 80.23.122.3

2. On the command-line, type:

   ```bash
   ansible all -m win_ping --extra-vars="ansible_user=username ansible_password=password ansible_connection=winrm ansible_winrm_server_cert_validation=ignore"
   ```

   If Green, it should be a successful ping.