### Jenkins 101

#### Lets create jenkins vm and setup entrie jenkins environemnt. 

~~~
lxc launch images:centos/7/amd64 jenkins
~~~

~~~
[root@lxd-demo-1 ~]# lxc list | grep -i jenk
| jenkins  | RUNNING | 10.139.68.51 (eth0) | fd42:7363:96b8:bb6a:216:3eff:fead:676b (eth0) | CONTAINER | 0         |
[root@lxd-demo-1 ~]# 
~~~

#### Get inside the container. 

~~~
[root@lxd-demo-1 ~]# lxc exec jenkins bash
[root@jenkins ~]# 
~~~

#### Install Jenkins. 

* Jenkins is a Java application, so the first step is to install Java. Run the following command to install the OpenJDK 8 package:

~~~
yum install java-1.8.0-openjdk-devel
~~~

* The next step is to enable the Jenkins repository. To do that, import the GPG key using the following curl command:

~~~
curl --silent --location http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo | sudo tee /etc/yum.repos.d/jenkins.repo
~~~

* And add the repository to your system with:

~~~
rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
~~~

* Once the repository is enabled, install the latest stable version of Jenkins by typing:

~~~
yum install jenkins
~~~

* After the installation process is completed, start the Jenkins service with:

~~~
systemctl start jenkins
~~~

* To check whether it started successfully run:

~~~
systemctl status jenkins
~~~

* Finally enable the Jenkins service to start on system boot.

~~~
systemctl enable jenkins
~~~

#### Now we are using a lxd container, lets add **PORT FORWARDING** to have access of this container from outside. 

#### On **LXD** node. 

~~~
iface=ens3
D_PORT=9080
CONTAINER_IP_AND_PORT=10.139.68.51:8080
sudo iptables -t nat -A PREROUTING -p tcp -i $iface --dport $D_PORT -j DNAT --to-destination $CONTAINER_IP_AND_PORT
~~~

* We should see below output: 

~~~
[root@lxd-demo-1 ~]# iptables -t nat -L -n -v | grep -i 8080
    6   360 DNAT       tcp  --  ens3   *       0.0.0.0/0            0.0.0.0/0            tcp dpt:9080 to:10.139.68.51:8080
[root@lxd-demo-1 ~]# 
~~~
  
### Lets get the access to **JENKINS**

![Image ipa](https://github.com/NileshChandekar/lxd_containers/blob/master/images/01111.png)


* You will get the **CREDENTIALS** here: 

~~~
[root@jenkins ~]# cat /var/lib/jenkins/secrets/initialAdminPassword
ad44b713c8cc49ad99b736fc99175679
[root@jenkins ~]# 
~~~

* Once you enter the credentials, you will get the below window, 

![Image ipa](https://github.com/NileshChandekar/lxd_containers/blob/master/images/01112.png)

* I am just going with the defauls [Install Suggested Plugins]

![Image ipa](https://github.com/NileshChandekar/lxd_containers/blob/master/images/01113.png)

* This will ^^ take some time to get it install, depends on the speed of your Internet. 

* Once that is done, you will get window ^^ 

![Image ipa](https://github.com/NileshChandekar/lxd_containers/blob/master/images/01114.png)

* Next Window [Instance Configuration]

![Image ipa](https://github.com/NileshChandekar/lxd_containers/blob/master/images/01115.png)

* Click Save anf Finish

![Image ipa](https://github.com/NileshChandekar/lxd_containers/blob/master/images/01116.png)

* Start Using Jenkins

![Image ipa](https://github.com/NileshChandekar/lxd_containers/blob/master/images/01117.png)

