# ansible-postgres
Install and configure PostgreSQL on an Ubuntu VM


### Create the VM

```bash
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


