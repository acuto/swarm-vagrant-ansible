# Provision a Docker Swarm Cluster with Vagrant and Ansible

### Automatically provision a Docker Swarm cluster composed of two managers and two workers

## Preconditions:

* Linux, Windows or MacOS host with at least 8GB RAM
* VirtualBox - https://www.virtualbox.org/
* Vagrant - https://www.vagrantup.com/

## Step 1: Pull the Vagrant Box

Use Bento's Ubuntu 16.04 box - https://app.vagrantup.com/bento/boxes/ubuntu-16.04. Enter a shell and type:

```sh
$  vagrant box add bento/ubuntu-16.04 --provider virtualbox
```

## Step 2: Run the Vagrantfile

Type the following commands:

```sh
$ cd Vagrant
$ vagrant up
```

## Step 3: Verify the Swarm Cluster

Access the swarm manager:

```sh
$ vagrant ssh swarm-master-1
```

Check cluster nodes:

```sh
vagrant@swarm-master-1:~$ docker node ls
```

## Step 4: Start a Sample Service

Run a service named helloworld and based on NGINX:

```sh
vagrant@swarm-master-1:~$ docker service create --replicas 1 --name helloworld --publish 80:80 nginx
```

Verify that the service is correctly listed:

```sh
vagrant@swarm-master-1:~$ docker service ls
```

The NGINX default page can be accessed at http://192.168.77.10/. Now check which node the container has been deployed to:

```sh
vagrant@swarm-master-1:~$ docker service ps helloworld
```

Scale up the service to 4 and verify its balancing on the available nodes:

```sh
vagrant@swarm-master-1:~$ docker service scale helloworld=4
vagrant@swarm-master-1:~$ docker service ps helloworld
```

## Step 5: Destroy the Service and the Docker Swarm

Stop the helloworld service:

```sh
vagrant@swarm-master-1:~$ docker service rm helloworld
```

Exit the swarm manager and destroy the cluster:

```sh
$ vagrant destroy --force
```
