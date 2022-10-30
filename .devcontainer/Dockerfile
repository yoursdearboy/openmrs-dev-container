FROM tomcat:7-jre8

ARG VERSION

RUN apt-get update \
 && apt-get install -y apt-transport-https \
 && mkdir -p /etc/apt/keyrings \
 && curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg \
 && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian stretch stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null \
 && apt-get update \
 && apt-get install -y \
    docker-ce-cli \
    git \
 && rm -rf /var/lib/apt/lists/*


ADD https://sourceforge.net/projects/openmrs/files/releases/OpenMRS_Platform_${VERSION}/openmrs.war/download /usr/local/tomcat/webapps/openmrs.war

COPY .devcontainer/setenv.sh /usr/local/tomcat/bin/setenv.sh

COPY .devcontainer/wait-for-it.sh /usr/local/tomcat/wait-for-it.sh
COPY .devcontainer/startup.sh /usr/local/tomcat/startup.sh

RUN sed -i '/Connector port="8080"/a URIEncoding="UTF-8" relaxedPathChars="[]|" relaxedQueryChars="[]|{}^&#x5c;&#x60;&quot;&lt;&gt;"' /usr/local/tomcat/conf/server.xml

RUN chmod +x /usr/local/tomcat/wait-for-it.sh
RUN chmod +x /usr/local/tomcat/startup.sh

CMD ["/usr/local/tomcat/startup.sh"]
