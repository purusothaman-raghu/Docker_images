From oraclelinux:7

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
