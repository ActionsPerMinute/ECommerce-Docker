FROM appdynamics/ecommerce-java:oracle-java7

# set timezone to UTC
RUN mv /etc/localtime /etc/localtime.bak
RUN ln -s /usr/share/zoneinfo/Etc/UTC /etc/localtime

RUN yum -y install tar
RUN yum -y install ntp
RUN yum -y install git
RUN yum -y install wget

# Install Maven
RUN (cd /usr/local; wget --quiet http://apache.arvixe.com/maven/maven-3/3.3.3/binaries/apache-maven-3.3.3-bin.tar.gz; tar -xzf apache-maven-3.3.3-bin.tar.gz)
RUN ln -s /usr/local/apache-maven-3.3.3/bin/mvn /usr/bin/mvn

# Environment vars: Tomcat
ENV TOMCAT_MAJOR_VERSION 8
ENV TOMCAT_MINOR_VERSION 8.0.14
ENV CATALINA_HOME /tomcat

# Install Tomcat
RUN curl -O -k https://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_MAJOR_VERSION}/v${TOMCAT_MINOR_VERSION}/bin/apache-tomcat-${TOMCAT_MINOR_VERSION}.tar.gz \
    && curl -O -k https://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_MAJOR_VERSION}/v${TOMCAT_MINOR_VERSION}/bin/apache-tomcat-${TOMCAT_MINOR_VERSION}.tar.gz.md5 \
    && md5sum apache-tomcat-${TOMCAT_MINOR_VERSION}.tar.gz.md5 \
    && tar zxf apache-tomcat-*.tar.gz \
    && rm apache-tomcat-*.tar.gz* \
    && mv apache-tomcat-${TOMCAT_MINOR_VERSION} tomcat
RUN cd ${CATALINA_HOME}/bin;chmod +x *.sh

# Build ECommerce-Angular
ADD ECommerce-FaultInjectionUI /ECommerce-FaultInjectionUI
RUN (cd ECommerce-FaultInjectionUI; mvn clean install; cp target/FaultInjectionUI-0.0.1-SNAPSHOT.war ${CATALINA_HOME}/webapps/faultinjection-ui.war)


ADD startup.sh /startup.sh
RUN chmod +x /startup.sh

WORKDIR /

# Note: This command should not return or the container will exit
CMD "/startup.sh"

EXPOSE 80
EXPOSE 8080
