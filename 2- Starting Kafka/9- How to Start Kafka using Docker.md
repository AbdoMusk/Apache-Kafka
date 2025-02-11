How to Start Kafka using Docker?
================================

Start Kafka with Docker and run on all systems.

* * *

Running Kafka on Docker has the benefit that this method is going to work for all operating systems including Mac, Windows and Linux. It also helps to switch Kafka versions easily by simply updating the underlying docker-compose file.

* * *

Pre-requisite: Install Docker & Docker Compose
----------------------------------------------

[](#Pre-requisite:-Install-Docker-&-Docker-Compose-0)

Install Docker
--------------

[](#Install-Docker-1)

You must install Docker with the steps mentioned below to work.

*   Install Docker Desktop on [Mac](https://docs.docker.com/desktop/mac/install/).
    
*   Install Docker Desktop on [Windows](https://docs.docker.com/desktop/windows/install/).
    
*   Install Docker on [Linux](https://docs.docker.com/engine/install/ubuntu/) (choose your distro on the lefthand side menu).
    

Installing Docker Compose
-------------------------

[](#Installing-Docker-Compose-2)

*   For Mac & Windows, if you have installed Docker Desktop, then Docker Compose is included as part of those desktop installs.
    
*   For Linux, [follow the steps here](https://docs.docker.com/compose/install/#install-compose) (and do all the steps)
    

* * *

How to run Kafka with Docker?
-----------------------------

[](#How-to-run-Kafka-with-Docker?-3)

Docker Compose is a tool for defining and running multi-container Docker applications. As we'll start each of the Kafka services, e.g, zookeeper, broker, etc. in a different Docker container, docker-compose is a perfect tool to do that.

To use docker-compose, we have to define the services that make up our Kafka deployment in `docker-compose.yml` so they can be run together in an isolated environment.

Go to the GitHub project - [https://github.com/conduktor/kafka-stack-docker-compose](https://github.com/conduktor/kafka-stack-docker-compose) and clone or download the project. It has docker-compose files for many configurations.

```
$ git clone https://github.com/conduktor/kafka-stack-docker-compose.git
Cloning into 'kafka-stack-docker-compose'...
remote: Enumerating objects: 551, done.
remote: Counting objects: 100% (219/219), done.
remote: Compressing objects: 100% (125/125), done.
remote: Total 551 (delta 145), reused 140 (delta 83), pack-reused 332
Receiving objects: 100% (551/551), 110.29 KiB | 15.76 MiB/s, done.
Resolving deltas: 100% (375/375), done.
```
Next, go to the folder `kafka-stack-docker-compose` and list the files.

.sh.batnothing

```
$ ls -l
total 60
-rw-rw-r-- 1 ec2-user ec2-user  4719 Nov 16 11:37 full-stack.yml
-rw-rw-r-- 1 ec2-user ec2-user 11345 Nov 16 11:37 LICENSE
-rw-rw-r-- 1 ec2-user ec2-user  8100 Nov 16 11:37 README.md
-rwxrwxr-x 1 ec2-user ec2-user  2150 Nov 16 11:37 test
-rw-rw-r-- 1 ec2-user ec2-user  4759 Nov 16 11:37 zk-multiple-kafka-multiple-schema-registry.yml
-rw-rw-r-- 1 ec2-user ec2-user  4209 Nov 16 11:37 zk-multiple-kafka-multiple.yml
-rw-rw-r-- 1 ec2-user ec2-user  2728 Nov 16 11:37 zk-multiple-kafka-single.yml
-rw-rw-r-- 1 ec2-user ec2-user  2946 Nov 16 11:37 zk-single-kafka-multiple.yml
-rw-rw-r-- 1 ec2-user ec2-user  1604 Nov 16 11:37 zk-single-kafka-single.yml
```

Note the file `zk-single-kafka-single.yml`. As the name suggests, we'll use it to launch a Kafka cluster with a single Zookeeper and a single broker.

Now, use this command to launch a Kafka cluster with one Zookeeper and one Kafka broker. Add `-d` flag to run it in the background.


```
docker-compose -f zk-single-kafka-single.yml up -d
```

Check to make sure both the services are running:

```
$ docker-compose -f zk-single-kafka-single.yml ps
 Name             Command            State                       Ports
-------------------------------------------------------------------------------------------
kafka1   /etc/confluent/docker/run   Up      0.0.0.0:9092->9092/tcp, 0.0.0.0:9999->9999/tcp
zoo1     /etc/confluent/docker/run   Up      0.0.0.0:2181->2181/tcp, 2888/tcp, 3888/tcp
```

Kafka is going to be exposed on your computer at `localhost:9092`

Running commands against our Kafka running on Docker
----------------------------------------------------

[](#Running-commands-against-our-Kafka-running-on-Docker-4)

To run Kafka commands against Docker, we have two options:

*   Run commands directly from within the Docker container of Kafka (using `docker exec`)
    
*   Run commands from our host OS (we must first install the binaries)
    

#### **Option 1: Running commands from within the Kafka docker container**

```
docker exec -it kafka1 /bin/bash
```

Then, from within the container, you can start running some Kafka commands (without `.sh`)

```
[appuser@kafka1 ~]$ kafka-topics --version
6.2.1-ccs (Commit:fa4bec046a2df3a6)
```

#### **Option 2: Running commands from outside your container**

For this, you must install the Java and the Kafka Binaries on your system:

*   instructions for [Mac](/kafka/how-to-install-apache-kafka-on-mac/) (follow the whole document except starting Kafka and Zookeeper)
    
*   instructions for [Linux](/kafka/how-to-install-apache-kafka-on-linux/) (follow the whole document except starting Kafka and Zookeeper)
    
*   instructions for [Windows](/kafka/how-to-install-apache-kafka-on-windows/) (follow the whole document except starting Kafka and Zookeeper)
    

One this is installed, the Kafka binaries should be in your path, and you will be good to issue your first Kafka command, for example `kafka-topics.sh`

Stopping Kafka on Docker
------------------------

[](#Stopping-Kafka-on-Docker-5)

Now that we've verified the installation, we can stop the services by stopping the respective containers:

```
$ docker-compose -f zk-single-kafka-single.yml stop
Stopping kafka1 ... done
Stopping zoo1   ... done  
```

To remove all the resources including the containers altogether, use `down` instead of `stop`:

```
$ docker-compose -f zk-single-kafka-single.yml down
Removing kafka1 ... done
Removing zoo1   ... done
Removing network kafka-stack-docker-compose_default
```
