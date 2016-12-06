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
