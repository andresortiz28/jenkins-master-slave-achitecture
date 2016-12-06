# Jenkins Master-Slave Architecture 

Andres Felipe Ortiz - 10207000
Universidad Icesi

### Description
The project deploys a master jenkins node which is the main node to do the Jobs, and also a slave jenkins node to do those kind of jobs.

This project is about an infrastructure automation using Docker and Docker-compose in order to create virtual containers to desploy Jenkins as continuos integration tool, to test a Github repository.

This code will allow to any developer to deploy automatically a virtual infrastructure containing:

* One master node of jenkins
* One slave node of jenkins to test a github repository inside a docker container.

This kind of infrastructure was deployed under a Linux based operating system, specifically ubuntu 14.04.

### Required Software:
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

### Deploy
Open a terminal, go to the directory where you cloned the repository, and deploy in this way:
```sh
docker-compose build
docker-compose up master_jk
```

### Verify
Open a browser and type http://172.17.0.1 to see the Jenkins dashboard.


### Manual Test

The following pictures show the process to do a manual test of the system, step by step:

Structure of the source code

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f1.png)

#### Step 1
Open a terminal, go to the source code folder, and type
```sh
docker-compose build
```
You can see the new docker images: "miniproyecto_master_jk" & "miniproyecto_slave_jk"

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f2.png)

#### Step 2
Up the master node of jenkins
```sh
docker-compose up master_jk
```

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f3.png)

#### Step 3
Open a browser and type the IP adress 172.17.0.1:80

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f6.png)

#### Step 4
Clic on "Manage Jenkins"

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f7.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f8.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f9.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f10.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f11.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f12.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f13.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f14.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f15.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f16.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f17.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f18.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f19.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f20.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f21.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f20.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f21.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f22.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f23.png)

![alt tag](https://github.com/andresort28/jenkins-master-slave-achitecture/blob/master/img/f24.png)


## Contribute
Github is all about contributions. If you think you can collaborate or improve this, please make sure you send me a pull request

## License
Copyright (c) 2016 [Andres Ortiz](http://www.andresfelipeortiz.com).  
Licensed under the MIT license.
