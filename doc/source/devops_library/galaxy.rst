.. _galaxy_config:

galaxy_config
@@@@@@@@@@@@@

**Prerequisite**   
1)python  
~~~
$ python -V
Python 2.7.3
~~~
2) Download galaxy zip 
~~~
wget https://github.com/galaxyproject/galaxy/archive/dev.zip
~~~
#### Steps to deploy on AWS 
1) Extract Downloaded folder
~~~
unzip dev.zip
~~~

2) Expose port to listen  
~~~
vi galaxy-dev/config/galaxy.ini
# The port on which to listen.
#port = 8080
port = 80
~~~

3)The address on which to listen.  By default, only listen to localhost (Galaxy
will not be accessible over the network).  Use '0.0.0.0' to listen on all
available network interfaces.
~~~
vi galaxy-dev/config/galaxy.ini
#host = 127.0.0.1
host = 0.0.0.0
~~~
4) set the host name to listen on all available network interfaces.
~~~
sudo vi /etc/hosts
0.0.0.0         ec2-54-70-149-21.us-west-2.compute.amazonaws.com
~~~
5) Start galaxy as daemon
~~~
sudo sh run.sh --daemon
sudo sh run.sh --stop-daemon
~~~