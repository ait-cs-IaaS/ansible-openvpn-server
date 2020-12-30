OpenVPN Server
=========

Ansible role for installing and configuring an OpenVPN server.
This role is adapted from [BastiPaeltz/ansible-openvpn](https://github.com/BastiPaeltz/ansible-openvpn).


Role Variables
--------------

| Variable name                                  | Type       | Default                                               | Description                                                                    |
| ---------------------------------------------- | ---------- | ----------------------------------------------------- | ------------------------------------------------------------------------------ |
| openvpn_server_config_template                 | path       | openvpn_server.conf.j2                                | The config template file                                                       |
| openvpn_server_iptables_config_template        | path       | openvpn_iptables_rules.v4.j2                          | The iptables rules template file                                               |
| openvpn_server_path                            | path       | /etc/openvpn                                          | Path to the OpenVPN config dir                                                 |
| openvpn_server_public_path                     | path       | {{ openvpn_server_path }}/server                      | Path to the public config dir                                                  |
| openvpn_server_private_path                    | path       | {{ openvpn_server_path }}/private                     | Path to the private config dir                                                 |
| openvpn_server_ta_key                          | path       | {{ openvpn_server_private_path }}/ta.key              | Path for the TA key file                                                       |
| openvpn_server_ca_cert                         | path       | {{ openvpn_server_public_path }}/ca.crt               | Path for the CA cert file                                                      |
| openvpn_server_ca_cert_src                     | path       |                                                       | Source path to the CA cert file to copy                                        |
| openvpn_server_cert                            | path       | {{ openvpn_server_public_path }}/server.crt           | Path for the server cert file                                                  |
| openvpn_server_cert_src                        | path       |                                                       | Source path to the server cert file to copy                                    |
| openvpn_server_key                             | path       | {{ openvpn_server_public_path }}/server.key           | Path for the server key file                                                   |
| openvpn_server_key_src                         | path       |                                                       | Source path to the server key file to copy                                     |
| openvpn_server_dhparams_location               | path       | {{ openvpn_server_private_path }}/dh.pem              | Path for the DH key                                                            |
| openvpn_server_dhparams_size                   | int        | 2048                                                  | Size of the DH key                                                             |
| openvpn_server_crl                             | path       | {{ openvpn_server_private_path }}/crl.pem             | Path to the CRL file                                                           |
| openvpn_server_tls_auth                        | bool       | false                                                 | If TLS auth should be used or not                                              |
| openvpn_server_auth                            | bool       | false                                                 | If auth should be used or not                                                  |
| openvpn_server_crl_verify                      | bool       | false                                                 | If he CRL file should be checked or not                                        |
| openvpn_server_verbosity                       | int        | 0                                                     | The logging verbosity for the server                                           |
| openvpn_server_cipher                          | str        | AES-256-CBC                                           | The cipher option for the server                                               |
| openvpn_server_data_ciphers                    | list[str]  | {{ openvpn_server_cipher }}                           | The list of data ciphers that should be available                              |
| openvpn_server_auth_digest                     | str        | SHA256                                                | The digest algorithm to use when auth is active                                |
| openvpn_server_tls_cipher                      | str        | \*                                                    | The list of TLS ciphers to support                                             |
| openvpn_server_instances                       | list[dict] |                                                       | List of OpenVPN server instances to run                                        |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.proto                | str        | udp                                                   | The protocol to use (udp/tcp)                                                  |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.port                 | int        | 1194                                                  | The port to bind to                                                            |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.mask                 | str        | 10.9.0.0 255.255.255.0                                | The network mask for the tunnel                                                |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.cidr                 | str        | 10.9.0.0/24                                | The network mask for the tunnel                                                |
| &nbsp;&nbsp;&nbsp;&nbsp;∟.server_extra_options | list[str]  | ['push "redirect-gateway def1"']                      | Extra options for the instance                                                 |
| openvpn_server_load_iptables_rules             | bool       | no                                                    | If the role should configure iptables or not                                   |
| openvpn_server_iptables_noflush                | bool       | no                                                    | If the iptables rules should be appended to or overwrite the current rules\*\* |
| openvpn_server_iptables_rules                  | path       | "{{ openvpn_server_path }}/openvpn_iptables_rules.v4" | Path for the iptables rules file                                               |
| openvpn_server_iptables_backup                 | path       | {{ openvpn_server_path }}/iptables_rules_backup.v4    | The path to backup the current iptables rules to                               |
| openvpn_server_iptables_nat_prerouting         | str        | ACCEPT                                                | NAT prerouting policy                                                          |
| openvpn_server_iptables_nat_input              | str        | ACCEPT                                                | NAT input policy                                                               |
| openvpn_server_iptables_nat_output             | str        | ACCEPT                                                | NAT output policy                                                              |
| openvpn_server_iptables_nat_postrouting        | str        | ACCEPT                                                | NAT postrouting policy                                                         |
| openvpn_server_iptables_filter_input           | str        | ACCEPT                                                | FILTER input policy                                                            |
| openvpn_server_iptables_filter_forward         | str        | ACCEPT                                                | FILTER forward policy                                                          |
| openvpn_server_iptables_filter_output          | str        | ACCEPT                                                | FILTER output policy                                                           |

\*TLS-DHE-DSS-WITH-AES-256-GCM-SHA384:TLS-DHE-RSA-WITH-AES-256-GCM-SHA384:TLS-DHE-RSA-WITH-AES-256-CBC-SHA256:TLS-DHE-DSS-WITH-AES-256-CBC-SHA256

\*\*Policies for default chains are always set even if `noflush` is `true`

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
      - role: openvpn-server
        vars:
          openvpn_server_instances:
            - proto: udp
              port: 1194
              mask: "10.9.0.0 255.255.255.0"
              cidr: "10.9.0.0/24"
              server_extra_options: ['push "redirect-gateway def1"']

```

License
-------

GPL
