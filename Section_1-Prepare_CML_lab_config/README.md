# Prepare Cisco Modeling Labs (CML) and Ansible

The first step to get the CML lab operational and start working with Ansible.  This directory generates the Ansible inventory files and Cisco Modeling Labs - Labfile.

## Folder Structure

Included in this directory are the following files and folders:

* ansible.cfg - Ansible configuration file containing default configuration for how we want Ansible to operate for the purposes of this lab.  For security purposes, in a production environment "host_key_checking" needs to be changed to True.
* inventory - This contains a lab_values.yaml file which will configure IP address for CML and the Ansible inventory.  More details below on configuration.
* processed_files - This directory is empty but will contain the processed templates that are created by the generate_CML_lab.yaml playbook.  The processed templates will result in an inventory.yaml file to allow Ansible to reach each host and the full lab configuration that can be imported to Cisco Modeling Labs
* templates - Jinja2 format templates with a few variables that are assigned from the lab_values.yaml file.
* generate_CML_lab.yaml - This is the Ansible playbook which will process the templates using the provided inventory and assign all variables to each file.  Output is saved to processed_files.

### A Note on altering inventory

The values listed below in lab_values.yaml can be altered for your environment.  The ubuntu_host_ens2 and ubuntu_host_ens2_gw variables in this lab point to an internet facing connection, and if changed need to be on a subnet that can reach the internet as well.  ENS2 is attached to the External Connector, operating in "bridge mode", that allows the ubuntu host to download necessary packages, clone this repository, and provide SSH access to run the playbooks. Your dns1 variable should use the DNS server IP of your local subnet, for home systems this is generally your router GW address or a public DNS like google on 8.8.8.8.  If ENS2 cannot reach the internet the cloud-config will fail to deploy and your lab will not operate.  The management and ubuntu_host_ens3 addresses can be changed as desired, so long as they are all in the same subnet.  You must assign the appropriate prefix mask for Ubuntu hosts that you change.

```
        cml_lab_title: "Practice Lab"
        cat8kv_r1_mgmt: 172.16.1.101
        cat8kv_r2_mgmt: 172.16.1.102
        cat9kv_sw1_mgmt: 172.16.1.103
        cat9kv_sw2_mgmt: 172.16.1.104
        n9kv_sw1_mgmt: 172.16.1.105
        ubuntu_host_ens2: "10.82.86.160/24"
        ubuntu_host_ens2_gw: "10.82.86.1"
        domain_name: ansible.lab
        dns1: "10.82.128.1"
        ubuntu_host_ens3: "172.16.1.100/24"
```

### Running the playbook

To execute the playbook run

```
ansible-playbook generate_CML_lab.yaml
```