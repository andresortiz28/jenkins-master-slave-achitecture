# Jenkins Master-Slave Architecture 

Andres Felipe Ortiz - 10207000
Universidad Icesi

### Description
This project is about an infrastructure automation using Docker and Docker-compose in order to create virtual containers to desploy Jenkins as continuos integration tool, to test a Github repository. The project deploys a master jenkins node which is the main node to do the Jobs, and also a slave jenkins node to do those kind of jobs.

This code will allow to any developer to deploy automatically a virtual infrastructure containing:

* One master node of jenkins
* One slave node of jenkins to test a github repository inside a docker container.

This kind of infrastructure was deployed under a Linux based operating system, specifically ubuntu 14.04.

### Required Software:
* Ubuntu 14.04 (Host PC)
* Docker
* Docker-Compose

In the sections below, instructions for executing the provisioning infrastructure are presented:

### Clone the repository
Clone the repository to any folder in your computer
```sh
git clone https://github.com/andresort28/jenkins-master-slave-achitecture.git 
```

### Download Docker images
Just pull 2 docker images from docker hub. Open a terminal a type the following:
```sh
docker pull jenkins
docker pull evarga/jenkins-slave
```

### Content

docker-compose.yml
```sh
version: '2'
services:
  master_jk:
    build: ./master_jk
    ports:
      - "80:8080"
  slave_jk:
    build: ./slave_jk 
```

master_jk/Dockerfile
```sh
FROM jenkins
MAINTAINER andresortiz28@hotmail.com

RUN /usr/local/bin/install-plugins.sh github
RUN /usr/local/bin/install-plugins.sh docker-plugin
RUN /usr/local/bin/install-plugins.sh saferestart

ENV JENKINS_MIRROR http://mirrors.jenkins-ci.org
ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false"
```

slave_jk/Dockerfile
```sh
FROM evarga/jenkins-slave
MAINTAINER andresortiz28@hotmail.com

ENV JENKINS_MIRROR http://mirrors.jenkins-ci.org
USER root

# --- Install the needed dependencies ---
RUN apt-get update
RUN apt-get install git -y
RUN apt-get install python -y
RUN apt-get install wget -y


# --- Install Python package manager and virtualenv package ---
RUN wget https://bootstrap.pypa.io/get-pip.py
RUN python get-pip.py
RUN pip install virtualenv


# --- Create a virtual environment for the project
WORKDIR /home/jenkins
RUN mkdir /home/jenkins/.virtualenvs
WORKDIR /home/jenkins/.virtualenvs
RUN virtualenv testproject
RUN . testproject/bin/activate


# --- Install requirements for the project in the virtualenv ---
WORKDIR /home/jenkins/workspace/test
RUN chmod -R 777 /home/jenkins/workspace/test
RUN pip install xmlrunner
RUN pip install unittest2
RUN pip install pytest
RUN pip install pytest-cov
RUN pip install flask
```

### Configure Docker API remote
In order to allow jenkins to comunicate to the Docker engine of the host machine, we need to make the following:
You have to edit the file /etc/init/docker.conf in the Host PC, and change a value as following:
```sh
DOCKER_OPTS="-H tcp://0.0.0.0:5050 -H unix:///var/run/docker.sock"
```
Then you have to restart docker
```sh
sudo service docker restart
```
It means, that any IP by the port 5050 could communicate with the docker engine.


### Deploy
Open a terminal, go to the directory where you cloned the repository, and deploy in this way:
```sh
docker-compose build
docker-compose up master_jk
```

### Verify
Open a browser and type http://172.17.0.1 to see the Jenkins dashboard.


## Manual Test

The following pictures show the process to do a manual test of the system, step by step:

Structure of the source code

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f1.png)

### Step 1
Open a terminal, go to the source code folder, and type
```sh
docker-compose build
```
You can see the new docker images: "miniproyecto_master_jk" & "miniproyecto_slave_jk"

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f2.png)

### Step 2
Up the master node of jenkins
```sh
docker-compose up master_jk
```

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f3.png)

### Step 3
Open a browser and type the IP adress 172.17.0.1:80 & Clic on "Manage Jenkins", and "Manage Plugins"

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f6.png)

You can see the docker, github and restart as installed plugins

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f7.png)

### Step 4
Go back to "Manage Jenkins", and clic on "Configure System". Then go down and clic on "Add new cloud" and clic on "Docker"

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f8.png)

Fill out the form as following, with the IP (Host PC) and PORT of Docker API Remote, which we have to configure before to do "docker-compose up master_jk".

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f9.png)

Add new credentials

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f10.png)

Add a Username and Password both like "jenkins", and clic on "Add"

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f11.png)

### Step 5
Then, you need to add a Docker template

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f12.png)

In this parte, you need to put the name of the Docker image of the slave node of jenkins created in the Step #1 called "miniproyecto_slave_jk", and complete the rest of the form as following:

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f13.png)

Just save it.

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f14.png)

### Step 6
Go back to the Dashboard, and clic on "New Item"

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f15.png)

Just type "test" as the name of the new item.

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f16.png)

Choose the same label of the Docker Template at the beginning.

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f17.png)

### Step 7
In this part, we are going to use a repository of git, just to compile and make a test. So, just copy the github link as following, and add the created credential.

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f18.png)

### Step 8
Choose to execute the test process in a Shell.

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f19.png)

In this parte, we need to write the code we want to execute to make the testing process and save it.
```sh
. $WORKSPACE/run_tests.sh
```

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f20.png)

### Step 9
Then, clic on "Build Now" to execute. Just wait.

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f21.png)

Awesome!
The job was executed with success.

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f22.png)

### Step 10
To view the results, just clic on "Console Output"

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f23.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f24.png)

### References
* https://github.com/jenkinsci/docker
* https://wiki.jenkins-ci.org/display/JENKINS/Docker+Plugin
* https://hub.docker.com/r/evarga/jenkins-slave/
* https://www.youtube.com/watch?v=0Tub1TUrmX8
* https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+with+Docker
* https://github.com/evarga/docker-images/blob/master/jenkins-slave/Dockerfile
* https://github.com/d4n13lbc/testproject


## Contribute
Github is all about contributions. If you think you can collaborate or improve this, please make sure you send me a pull request

## License
Copyright (c) 2016 [Andres Ortiz](http://www.andresfelipeortiz.com).  
Licensed under the MIT license.
