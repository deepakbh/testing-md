# Description
This project has **Ansible** code for performing '*Post OS-installation*' configurations on **Electric Cloud**  servers running **Ubuntu 16.04**.

[Skip to Running the playbook](#running-the-playbooks)

## Environment
|**Table 1**        |                            |
|-------------------|----------------------------|
|**Hardware**       | iX Systems (Supermicro)    |
|**BIOS Vendor**    | Aptio (American Megatrends)|
|**BIOS Version**   | 3.1                        |
|**OS Version**     | Ubuntu 16.04.3             |
|**Ansible version**| 2.3.3.0 or later           |

## Pre-requiste
1. Server should have OS deployed (Ubuntu 16.04.3)
2. Server should be on network
3. Hostname should be registered in DNS
4. SSH should be open for root

## Installation methods
1. Clone this repo
```
$ git clone git@ssd-git.juniper.net:cloudops/ea_ubuntu16.git
```
2. Use pre-installed repo on **qnc-eng-ansible**
```
[user@qnc-eng-ansible ~:]$ cd /home/projects/all/ea_ubuntu16
[user@qnc-eng-ansible /home/projects/all/ea_ubuntu16]$
```

   Run all commands from the above path ```/home/projects/all/ea_ubuntu16```
## Running the playbooks
Running playbooks against one or more servers is easy. Just follow the steps.

### Step 1. Edit Inventory file to add your server
Use your favorite editor to edit the inventory file (hosts.ini) and add your host(s) to
appropriate host-group(s).
As of now there are **four** different host-groups as listed below,
two for each GEO.
1. **Bangalore**
* electric_cloud_ub16_emake_servers_bng
* electric_cloud_ub16_agent_servers_bng
2. **Quincy**
* electric_cloud_ub16_emake_servers_qnc
* electric_cloud_ub16_agent_servers_qnc

You need to figure out the GEO and type (emake or agent) for each server and add them under the appropriate host-group(s).
### Step 2. Verify with --list-hosts
Before you run the playbook it is always a good idea to check if the servers that you just added are showing up in with the ```--list-hosts``` option.  
```
$ ./electric_cloud_ub16.yml -i hosts.ini --list-hosts --limit qnc-ea-agt-104*

playbook: ./electric_cloud_ub16.yml

  play #1 (electric_cloud_ub16_servers): electric_cloud_ub16_servers    TAGS: []
    pattern: [u'electric_cloud_ub16_servers']
    hosts (4):
      qnc-ea-agt-104d
      qnc-ea-agt-104a
      qnc-ea-agt-104c
      qnc-ea-agt-104b
$
```
Note the use of ```---limit``` (```-l``` is its short form) option. This helps in listing the hosts the you are intersted in.  
You can separate the list of hosts with commas(,) for example  
```
$ ./electric_cloud_ub16.yml -i hosts.ini --list-hosts -l qnc-emake-32a,qnc-emake-32b,qnc-ea-agt-104*
```  
### Step 3. Run the playbook
Once you are sure of the list of servers against which the playbook would run, use the following command to run the playbook.  
**Note:** You should be able to login to the target server with root (either with password or with ssh-keys) from your Ansible machine, before you run the playbook
```
$ ./electric_cloud_ub16.yml -i hosts.ini --list-hosts --limit qnc-ea-agt-104a
SSH password:
```
Once the playbook run is complete, the target server(s) would have the desired configurations in place and the requisite packages installed.
**Note:** The playbook is idempotent, i.e. multiple runs of the playbook would result in same state of the target server(s).
