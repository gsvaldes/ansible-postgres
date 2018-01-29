# ansible-postgres
Install and configure PostgreSQL on an Ubuntu VM

*In part based on Ansible Up and Running, updated for Ansible 2.0*
*http://www.ansiblebook.com/*


### Checkout project and create the VM


```bash
$ mkdir ansible-postgres
$ cd ansible-postgres
$ git clone https://github.com/gsvaldes/ansible-postgres.git .
$ vagrant up
```
### Set up SSH Keys

# on host machine
```bash
$ cd ~/.ssh
```

```bash
$ ssh-keygen -t rsa -b 4096 -C "vagrant@192.168.87.41"
Enter file in which to save the key (~/.ssh/id_rsa): id_rsa_ubuntu_postgres
$ ssh-copy-id -i id_rsa_ubuntu_postgres.pub vagrant@192.168.87.41
```

Note:: if you get the following error

```bash
ERROR: Host key verification failed.
```

Try:
```bash
$ ssh-keygen -R 192.168.87.41
```
and then re-run 

```bash
$ .ssh valdesgs$ ssh-copy-id -i id_rsa_ubuntu_postgres.pub vagrant@192.168.87.41
vagrant@192.168.87.41's password:     # default password is vagrant
```

On ubuntu_postgres
```bash
$ chmod 700 ~/.ssh
$ chmod 600 ~/.ssh/authorized_keys
```


Back on your host machine add the followiing to the .ssh/config file
Host 192.168.87.41
  IdentityFile ~/.ssh/id_rsa_ubuntu_postgres

Now verify that you can access the remote machine via ssh without a password
Note that you will still be asked for your passphrase if you entered one during the ssh-keygen step

```bash
$ ssh vagrant@192.168.87.41
```

### Create virtual env and install requirements
```bash
$ cd ansible-postgres
$ virtualenv venv
$ source venv/bin/activate
$ pip install -r requirements.txt
```

# Create the secrets.yml file
```bash
$ cp _secrets_template.yml secrets.yml
```
add a database password to the secrets.yml file


### run the ansible playbook
```bash
$ ansible-playbook postgres.yml
```


### Configure postgres to accept connections from host machine
The following will open up the postgres server to any connection, however we have it installed on
a VM that only accepts connections from the host machine.  This is not how we would configure 
these files for a production db server.

add the following line to the pg_hba.conf file in etc/postgresql/9.3/main

```
host     all             all             0.0.0.0/0               trust
```

uncomment the following line in postgresql.conf

```
#listen_addresses = 'localhost'		# what IP address(es) to listen on;
```

and change it to 

```
listen_addresses = '*'		        # what IP address(es) to listen on;
```

After making the changest to these files, you will need to restart postgres for them to take effect
You can either do this manually in the VM, or re-run the postgres.yml playbook.


### TODO
1. As of July 2016, the PostgreSQL version installed from the apt-repository is 9.3, update the playbook to install 9.5
2. Automatically update pg_hba.conf and postgresql.conf via the playbook


