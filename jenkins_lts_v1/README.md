Example:

docker run  -p 5000:5000 -p 8080:8080 -v jenkins_data:/opt/jenkins --name jenkins_Server jenkins_lts:v1

Jenkins url: http://<Ip_Address>:8080/jenkins

============================================================
DockerFile:
===========================================================

FROM oraclelinux:7

##List Yum repositories
#RUN yum repolist

##Add/Remove the YUM Repositories as Needed

#RUN yum -y update 

##To Install Oracle Java
RUN mkdir -p /opt/tools/java
COPY ./jdk-11.0.6_linux-x64_bin.tar.gz  /opt/tools/java
WORKDIR /opt/tools/java
RUN tar -xzf jdk-11.0.6_linux-x64_bin.tar.gz ; rm -rf jdk-11.0.6_linux-x64_bin.tar.gz; /opt/tools/java/jdk-11.0.6/bin/java --version

##User to Run Tomcat & Jenkins
RUN groupadd -r jenkins; useradd -r -g jenkins -m -d /opt/jenkins jenkins

##To install Tomcat
ENV JAVA_HOME=/opt/tools/java/jdk-11.0.6
COPY ./apache-tomcat-9.0.30.tar.gz  /opt
WORKDIR /opt
RUN tar -xzf apache-tomcat-9.0.30.tar.gz;  rm -f apache-tomcat-9.0.30.tar.gz; /opt/apache-tomcat-9.0.30/bin/version.sh; chown -R jenkins:jenkins /opt/apache-tomcat-9.0.30

##To add Jenkins.war file
COPY ./jenkins.war /opt/apache-tomcat-9.0.30/webapps

##To start Tomcat & Jenkins
EXPOSE 8080
EXPOSE 5000 

##Yum install to fix some dependencies
RUN yum -y install freetype fontconfig; yum clean all


##Starting Tomcat & Jenkins
USER jenkins
WORKDIR /opt/jenkins
ENTRYPOINT ["/opt/apache-tomcat-9.0.30/bin/catalina.sh" , "run"]


=============================================================
docker system info output of server used to build this image.
=============================================================
Client:
 Debug Mode: false

Server:
 Containers: 3
  Running: 2
  Paused: 0
  Stopped: 1
 Images: 74
 Server Version: 19.03.8
 Storage Driver: overlay2
  Backing Filesystem: <unknown>
  Supports d_type: true
  Native Overlay Diff: true
 Logging Driver: json-file
 Cgroup Driver: systemd
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: pending
  NodeID: su7knqcy31s0vz7y4llsymyb3
  Is Manager: false
  Node Address: 192.168.57.1
  Manager Addresses:
   192.168.57.51:2377
   192.168.57.52:2377
   192.168.57.53:2377
 Runtimes: runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc version: dc9208a3303feef5b3839f4323d9beb36df0a9dd
 init version: fec3683
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 3.10.0-1062.18.1.el7.x86_64
 Operating System: CentOS Linux 7 (Core)
 OSType: linux
 Architecture: x86_64
 CPUs: 4
 Total Memory: 6.563GiB
 Name: MyPC
 ID: OBHC:OEM2:6X3H:HSYE:XNQ2:MNAQ:3VF5:BNLM:WU56:4BQR:YYLS:SZBA
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Username: purusothaman
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
