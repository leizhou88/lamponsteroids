AWSCLI role
===========

This role will install AWS CLI command line tool for non-root users on __RHEL 7__.

Normally it is easiest to install AWS CLI with PIP, but this is not that simple on RHEL 7.

* RHSCL limits ways that pip can be initialized and used, because you have to enable the software collection at the beginning of every user session. (https://access.redhat.com/solutions/527703)

* Also tried using EPEL as an alternative but still issues - the 1st task below may hang without obvious reason on RHEL 7.6.x. (https://packaging.python.org/guides/installing-using-linux-tools/)
 
* Finally I chose to use the Amazon "Bundled Installer" and install without Sudo. This approach installed AWS CLI for each specified user locally without altering any system environment variable. 
It is the most portable and clean way of managing the AWS CLI installation.
[Amazon instruction link](https://docs.aws.amazon.com/cli/latest/userguide/install-bundle.html)

## Usage

Use `/playbooks/install-awscli.yml` as an example. 

* First define the following AWS credential in an ansible vault file: 

    * `awscli_aws_access_key_id`
    * `awscli_aws_secret_access_key`

    For safety reasons, do not store the vault file in code repository. You can store it under `~/.ansible/vault` for example. Make sure the file is encrypted with `ansible-vault` command.

* Second, create basic configuration in your vars file for the playbook (see `vars/my-vars.yml` for example).

    * `awscli_default_region_name`
    * `awscli_default_output_format`

After above configuration, the following command can be used to start AWS CLI installation: 

`ansible-playbook -i /etc/ansible/hosts playbooks/install-awscli.yml --ask-vault-pass`

The above playbook example assumes you have already created and encrypted an Ansible vault file: 

`~/.ansible/vault/credentials.yml`


## Note

As per AWS best practices, you should not use root access ID and key as it exposes root access to your entire AWS infrastructure. In stead, consider use IAM to create one or more normal users and assign proper groups/roles for them. 

After installed AWS CLI, the user can use `aws config` to enter the credential that they want to use. 


