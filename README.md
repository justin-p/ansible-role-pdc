# ansible-pdc

This role will create a brand new Primary Domain Controller with a Active Directory Domain/Forest. No hardening is applied.

Works on

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012R2

## Requirements

- `python3-winrm` (`pywinrm`) is needed for WinRM.

## Role Variables

All variables listed in `default/main.yml` reference variables from `vars/main.yml`.  
If you want to change any variables overwrite the ones listed in `vars/main.yml`.

### `vars/main.yml`

| Variable                         | Default value                       |
|:---------------------------------|:------------------------------------|
| pdc_admin_username               | administrator                       |
| pdc_admin_password               | P@ssw0rd!                           |
| pdc_admin_password_never_expires | yes                                 |
| pdc_admin_groups                 | ["Administrators","Domain Admins","Domain Users","Enterprise Admins","Group Policy Creator Owners","Schema Admins"] |
| pdc_domain                       | ad.example.test                     |
| pdc_netbios                      | TEST                                |
| pdc_domain_path                  | dc=ad,dc=example,dc=test            |
| pdc_domain_safe_mode_password    | P@ssw0rd!                           |
| pdc_domain_functional_level      | Default                             |
| pdc_forest_functional_level      | Default                             |
| pdc_delayed_services             | ["WinRM"]                           |
| pdc_required_psmodules           | ["ActiveDirectoryDsc"]              |
| pdc_required_features            | ["AD-domain-services","DNS"]        |

## Dependencies

N/A

## Example Playbook

    - hosts: primarydomaincontroller
      roles:
         - { role: justin_p.pdc }

## Local Development

This role includes a Vagrantfile that will spin up a local Windows Server 2019 VM in Virtualbox.  
After creating the VM it will automatically run our role.

### Development requirements

`pip3 install pywinrm`

#### Usage

- Run `vagrant up` to create a VM and run our playbook
- Run `vagrant provision` to reapply our playbook
- Run `vagrant destroy -f && vagrant up` to recreate the VM and run our playbook.
- Run `vagrant destroy` to remove the VM.

## License

MIT

## Authors

- Justin Perdok ([@justin-p](https://github.com/justin-p/)), Orange Cyberdefense
