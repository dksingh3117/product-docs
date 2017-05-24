.. _ensembl_config:

ensembl_config
@@@@@@@@@@@@@@

**Prerequisite**  
1) Git   
~~~
sudo yum install git
~~~
2)	Docker

To install Docker on an Amazon Linux instance

Launch an instance with the Amazon Linux AMI.

Connect to your instance.
~~~
ssh -i "testvm-key.pem" ec2-user@ec2-35-165-60-101.us-west-2.compute.amazonaws.com
~~~
Update the installed packages and package cache on your instance.
~~~
[ec2-user ~]$ sudo yum update -y
~~~
Install Docker.
~~~
[ec2-user ~]$ sudo yum install -y docker
~~~
Start the Docker service.

~~~
[ec2-user ~]$ sudo service docker start
Starting cgconfig service:                                 [  OK  ]
Starting docker:	                                       [  OK  ]
~~~

Add the ec2-user to the docker group so you can execute Docker commands without using sudo.
~~~
[ec2-user ~]$ sudo usermod -a -G docker ec2-user
~~~

Log out and log back in again to pick up the new docker group permissions.

Verify that the ec2-user can run Docker commands without sudo.

~~~
[ec2-user ~]$ docker info
Containers: 2
Images: 24
Storage Driver: devicemapper
 Pool Name: docker-202:1-263460-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 702.3 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 1.864 MB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.89-RHEL6 (2014-09-01)
Execution Driver: native-0.2
Kernel Version: 3.14.27-25.47.amzn1.x86_64
Operating System: Amazon Linux AMI 2014.09
~~~

###docker-ensembl


Dockerfile to create a container image with the EnsEMBL Perl APIs and tools installed

Make docker-ensembl directory 

~~~
mkdir docker-ensembl
~~~

~~~
vi Dockerfile

# Dockerfile to build Bio-Linux 8
#
# VERSION 0.1

# use vanilla ubuntu base image
FROM ubuntu

# maintained by me
MAINTAINER Deepak Singh<dksingh3117@gmail.com>

# update aptitude and install some required packages
RUN apt-get update && apt-get -y install build-essential cpanminus curl emacs git manpages perl perl-base perlbrew tmux vim wget

# create ensembl user
RUN useradd -r -m -U -d /home/ensembl -s /bin/bash -c "EnsEMBL User" -p '' ensembl
RUN usermod -a -G sudo ensembl

# turn off password requirement for sudo groups users
#RUN sed -i "s/^\%sudo\tALL=(ALL:ALL)\sALL/%sudo ALL=(ALL) NOPASSWD:ALL/"> /etc/sudoers
RUN apt-get update && \
      apt-get -y install sudo

# change user to ensembl
USER ensembl

# set HOME environment variable
ENV HOME /home/ensembl

# change working directory
WORKDIR $HOME

# install ensembl dependencies
RUN sudo apt-get -y install mysql-client libmysqlclient-dev libssl-dev
RUN sudo cpanm DBI DBD::mysql
RUN sudo chown -R ensembl:ensembl $HOME/.cpanm

# clone git repositories
RUN mkdir -p src
WORKDIR $HOME/src
RUN git clone https://github.com/bioperl/bioperl-live.git
WORKDIR $HOME/src/bioperl-live
RUN git checkout bioperl-release-1-6-901
WORKDIR $HOME/src
RUN git clone https://github.com/Ensembl/ensembl-git-tools.git
ENV PATH $HOME/src/ensembl-git-tools/bin:$PATH
RUN git ensembl --clone api
RUN git clone https://github.com/Ensembl/ensembl-tools.git

# update bash profile
RUN echo >> $HOME/.profile && \
echo '# set ensembl perl libraries' >> $HOME/.profile && \
echo PERL5LIB=\$PERL5LIB:$HOME/src/bioperl-live >> $HOME/.profile && \
echo PERL5LIB=\$PERL5LIB:$HOME/src/ensembl/modules >> $HOME/.profile && \
echo PERL5LIB=\$PERL5LIB:$HOME/src/ensembl-compara/modules >> $HOME/.profile && \
echo PERL5LIB=\$PERL5LIB:$HOME/src/ensembl-variation/modules >> $HOME/.profile && \
echo PERL5LIB=\$PERL5LIB:$HOME/src/ensembl-funcgen/modules >> $HOME/.profile && \
echo export PERL5LIB >> $HOME/.profile && \
echo >> $HOME/.profile && \
echo '# set ensembl tools in path' >> $HOME/.profile && \
echo PATH=$HOME/src/ensembl-git-tools/bin:\$PATH && \
echo PATH=$HOME/src/ensembl-tools/scripts/assembly_converter:\$PATH >> $HOME/.profile && \
echo PATH=$HOME/src/ensembl-tools/scripts/id_history_converter:\$PATH >> $HOME/.profile && \
echo PATH=$HOME/src/ensembl-tools/scripts/region_reporter:\$PATH >> $HOME/.profile && \
echo PATH=$HOME/src/ensembl-tools/scripts/variant_effect_predictor:\$PATH >> $HOME/.profile && \
echo export PATH >> $HOME/.profile

# setup environment
ENV PERL5LIB $PERL5LIB:$HOME/src/bioperl-live:$HOME/src/ensembl/modules:$HOME/src/ensembl-compara/modules:$HOME/src/ensembl-variation/modules:$HOME/src/ensembl-funcgen/modules
ENV PATH $HOME/src/ensembl-tools/scripts/assembly_converter:$HOME/src/ensembl-tools/scripts/id_history_converter:$HOME/src/ensembl-tools/scripts/region_reporter:$HOME/src/ensembl-tools/scripts/variant_effect_predictor:$PATH

# change working directory
WORKDIR $HOME

# create script to test connection to database
RUN echo '#!/usr/bin/env perl' > ensembl_test_db_conn.pl && \
echo "use strict; use warnings; use Bio::EnsEMBL::Registry; my \$registry = 'Bio::EnsEMBL::Registry'; \$registry->load_registry_from_db(-host => 'ensembldb.ensembl.org', -user => 'anonymous', -verbose => '1');" >> ensembl_test_db_conn.pl
RUN chmod a+x ensembl_test_db_conn.pl
~~~

Then `cd docker-ensembl` and type:

```
docker build -t dkmaven/docker-ensembl .
```

To run type:

```
docker run -it dkmaven/docker-ensembl bash
```

To test EnsEMBL is working type:

```
perl $HOME/ensembl_test_db_conn.pl
```

Debugging Ensembl API Connections  
1). Use ensembl/misc-scripts/ping_ensembl.pl

To ping our UK server but with a different species
~~~
> perl ensembl/misc-scripts/ping_ensembl.pl -species pig
~~~

2). Try Connecting to Ensembl Using a MySQL Client
~~~
if you can connect to our MySQL server using a client then the issue should be in your Perl or Ensembl setup.

mysql --host=ensembldb.ensembl.org --port=3306 --user=anonymous

  Welcome to the MySQL monitor.  Commands end with ; or \g.
  Your MySQL connection id is 4292641
  Server version: 5.1.34-log Source distribution

  Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

  Oracle is a registered trademark of Oracle Corporation and/or its
  affiliates. Other names may be trademarks of their respective
  owners.

  Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

  mysql>
~~~

