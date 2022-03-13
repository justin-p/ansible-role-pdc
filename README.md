# ansible-role-pdc


[![Ansible Role Name](https://img.shields.io/ansible/role/51179?label=Role%20Name&logo=ansible&style=flat-square)](https://galaxy.ansible.com/justin_p/pdc)
[![Ansible Quality Score](https://img.shields.io/ansible/quality/51179?label=Ansible%20Quality%20Score&logo=ansible&style=flat-square)](https://galaxy.ansible.com/justin_p/pdc)
[![Ansible Role Downloads](https://img.shields.io/ansible/role/d/51179?label=Ansible%20Role%20Downloads&logo=ansible&style=flat-square)](https://galaxy.ansible.com/justin_p/pdc)
[![Github Actions](https://img.shields.io/github/workflow/status/justin-p/ansible-role-pdc/CI?label=Github%20Actions&logo=github&style=flat-square)](https://github.com/justin-p/ansible-role-pdc/actions)

This role will create a brand new Primary Domain Controller with a Active Directory Domain/Forest. No hardening is applied.

Based of the work done by [@jborean93](https://github.com/jborean93) in [jborean93/ansible-windows](https://github.com/jborean93/ansible-windows)

Works on

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012R2

## Requirements

- `python3-winrm` (`pywinrm`) is needed for WinRM.

## Role Variables

`defaults/main.yml`

| Variable                      | Description                                                                                                                                                                                                                                    | Default value                                                                                                                                                   |
| :---------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| pdc_administrator_username    | Settings this to Built-in Administrator account ensure that we know the password of NETBIOS\Administrator. 9/10 times you should leave this to the default value.                                                                              | Administrator                                                                                                                                                   |
| pdc_administrator_password    | The password of Built-in Administrator account. This password (if pdc_administrator_username left to the default value) will become the password of NETBIOS\Administrator. Change this to a strong password.                                   | P@ssw0rd!                                                                                                                                                       |
| pdc_dns_nics                  | The name of the ethernet adapter to setup DNS on. Defaults to wildcard. 9/10 times you should leave this to the default value.                                                                                                                 | \*                                                                                                                                                              |
| pdc_dns_servers               | The DNS server to use on pdc_dns_nics. Defaults to `{{ ansible_host }}`. 9/10 times you should leave this to the default value.                                                                                                                | {{ ansible_host }}                                                                                                                                              |
| pdc_domain                    | The Domain of the new Active Directory Forest. For testing\lab purposes it's recommend to use [ad.domain.test](https://www.wikiwand.com/en/.test). For production it's recommend to use a existing domain with a ad subdomain: `ad.domain.tld` | ad.example.test                                                                                                                                                 |
| pdc_netbios                   | The NetBIOS of the new Active Directory Forest. Change this depending on your needs.                                                                                                                                                           | TEST                                                                                                                                                            |
| pdc_domain_path               | The Distinguished Name of the domain. This should match the value given in pdc_domain (Example: `dc=ad,dc=domain,dc=test`)                                                                                                                     | dc=ad,dc=example,dc=test                                                                                                                                        |
| pdc_domain_safe_mode_password | The Domain Safe Mode password. Change this to a strong password.                                                                                                                                                                               | P@ssw0rd!                                                                                                                                                       |
| pdc_domain_functional_level   | Specifies the domain functional level of the first domain in the creation of a new forest. The domain functional level cannot be lower than the forest functional level, but it can be higher. Change this depending on your needs.            | Default ([Windows2008R2](https://github.com/MicrosoftDocs/windows-powershell-docs/blob/master/docset/windows/addsdeployment/Install-ADDSForest.md#-domainmode)) |
| pdc_forest_functional_level   | Specifies the forest functional level for the new forest. The default forest functional level in Windows Server is typically the same as the version you are running. Change this depending on your needs.                                     | Default ([Windows2008R2](https://github.com/MicrosoftDocs/windows-powershell-docs/blob/master/docset/windows/addsdeployment/Install-ADDSForest.md#-forestmode)) |
| pdc_required_psmodules        | PowerShell/DSC modules to install from the PSGallery. Always make sure to include `ActiveDirectoryDsc`for the `WaitForAD`-check. 9/10 times you should leave this to the default value.                                                        | [xPSDesiredStateConfiguration, NetworkingDsc, ComputerManagementDsc, ActiveDirectoryDsc]                                                                        |
| pdc_required_features         | Windows Features that should be installed on the Domain Controller. Defaults to AD-domain-services and DNS. 9/10 times you should leave this to the default value.                                                                             | ["AD-domain-services", "DNS"]                                                                                                                                   |
| pdc_desired_dns_forwarders    | The desired DNS Forwarders for the PDC. Defaults to Google DNS. Change this depending on your needs.                                                                                                                                           | ["8.8.8.8", "8.8.4.4"]                                                                                                                                          |

## Dependencies

- WinRM on the windows host should configured for Ansible.
- justin_p.posh5
- justin_p.wincom

## Example Playbook

    - hosts: primary_domain_controller
      roles:
        - role: justin_p.posh5
        - role: justin_p.wincom
        - role: justin_p.pdc
          vars:
           pdc_administrator_password: Str0ngPassw0rd01!

See https://github.com/justin-p/ansible-role-pdc/blob/master/tests/inventory.yml for an example inventory.

## Local Development

This role includes a Vagrantfile that will spin up a local Windows Server 2019 VM in Virtualbox.  
After creating the VM it will automatically run our role.

### Development requirements

`pip3 install pywinrm`

#### Usage

- Run `vagrant up` to create a VM and run our role.
- Run `vagrant provision` to reapply our role.
- Run `vagrant destroy -f && vagrant up` to recreate the VM and run our role.
- Run `vagrant destroy` to remove the VM.

## License

MIT

## Authors

- Justin Perdok ([@justin-p](https://github.com/justin-p/)), Orange Cyberdefense

## Contributing

Feel free to open issues, contribute and submit your Pull Requests. You can also ping me on Twitter ([@JustinPerdok](https://twitter.com/JustinPerdok)).
