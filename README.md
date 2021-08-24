## Overview

This repo houses Ansible playbooks used the AnsibleFest 2021 session on Azure app automation with Ansible.

## Usage

Plays can be run as follows:
```bash
ansible-playbook deploy_container_app.yml --e "az_tenant_id=xxx"
ansible-playbook delete_stale_environments.yml -v
```

## Setup

1. Install the Ansible Galaxy Azure collection and dependencies:
	```bash
	ansible-galaxy collection install -r collections/requirements.yml
	pip3 install -r ~/.ansible/collections/ansible_collections/azure/azcollection/requirements-azure.txt
	```

### Configure authentication

You have two options for authentication: using your Azure AD user or setting up a service principal.

#### Authentication with Azure AD user

Configure the following environment variables. If you want the variables the persist for every session, place the following in your `~/.bashrc` file. Otherwise you can run them as commands and they will take effect only for the current shell session.
```bash
export AZURE_AD_USER="xxx@rbfcu.org"
export AZURE_PASSWORD="your pass"
export AZURE_SUBSCRIPTION_ID="find in azure portal or use output of 'az account show' to find id"
```

#### Authentication with Azure service principal

1. Create a service principal (SP) following the [Microsoft doc](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal).
	- You will need to have an Azure admin ensure the SP has appropriate role(s) assigned for the subscription. Typically the "Contributor" role is assigned so the SP can create any type of resource.
1. Configure credentials either through a credentials file or using environment variables, using the following values:
	- Client ID: AKA Application ID for the service principal.
	- Tenant ID: AKA Directory ID for the service principal.
	- Secret: Client secret value for service principal (app registration).
	- Subscription ID: The subscription ID for Azure. Can be found in the Azure portal or from output of `az account show` if using [azure-cli](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

##### Authenticate service principal using a credentials file

Create the credentials file at `$HOME/.azure/credentials` and include the following in it:
```
[default]
client_id=xxx
tenant=xxx
secret=xxx
subscription_id=xxx
```

##### Authenticate service principal using environment variables

Use the following environment variables:
```bash
export AZURE_CLIENT_ID="xxx"
export AZURE_TENANT="xxx"
export AZURE_SECRET="xxx"
export AZURE_SUBSCRIPTION_ID="xxx"
```

## Resources:
* [Azure Ansible overview/setup](https://docs.ansible.com/ansible/latest/scenario_guides/guide_azure.html).
* [Azure Ansible modules](https://docs.ansible.com/ansible/2.9/modules/list_of_cloud_modules.html#azure).
* [Sample Ansible Playbooks for Azure](https://github.com/Azure-Samples/ansible-playbooks).
