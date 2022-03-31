# Jenkins CICD Pipeline to Build a Docker Image with Splunk Integration

Description
Integrating Jenkins with Docker provides a robust CICD method for building and packaging runnable containers. Splunk can be used to monitor and assess their operational performance, through its support for enterprise-grade monitoring, logging, and diagnostics collection.

In this lab, I launched a Jenkins and Splunk CICD and monitoring environment using Docker containers on a provided EC2 instance. I then configured a Jenkins build pipeline to build, compile, and package a sample Java servlet web application into a runnable Docker image. The Jenkins build process creates the Docker image using a Dockerfile. The build process integrates Splunk logging into the Docker image, by installing the Splunk Forwarding Agent at build time. At runtime, the docker container will then have the ability to publish runtime logging information into the Splunk service.

---

Launched the Jenkins and Splunk docker environment.

```
docker pull jenkins/jenkins:lts
docker-compose up -d && docker-compose logs -f
```

![](/images/1.png)

![](/images/2.png)

Confirmed that the Jenkins, Splunk, and Socat docker containers were up and running:

![](/images/3.png)

After installing and configuring Jenkins, configured the Gradle and Docker tools required to compile a sample Java servlet web application into a WAR (Web application ARchive) artifact file, and then a docker image that uses Tomcat to serve the WAR artifact file created in the first build step. Configured the Gradle and Docker tools to be automatically installed at build time by Jenkins.

Created a new build pipeline job that used Docker to compile and build a new docker image containing the newly created deployable WAR file.

![](/images/6.png)

![](/images/7.png)

Used the _docker images_ command to confirm that the Jenkins build pipeline had successfully built

![](/images/5.png)

Launched the webapp docker container instance by running the command:

```
docker run --net lab -e CONTAINER_NETWORK=lab -e CONTAINER_SOCAT_ENABLED=true --name webapp -d -p 9999:8080 deserie/webapp:latest
```

Queried the splunk docker container assigned private IP address:

```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' splunk
```

Confirmed that the Splunk Forwarding Agent had been able to form a network connection to the Splunk docker container by executing the command:

```
docker exec -it webapp grep "INFO  TcpOutputProc - Connected to idx=" /opt/splunkforwarder/var/log/splunk/splunkd.log
```

Examined the Splunk Forwarder outputs.conf file (instructs the forwarder how to connect to the Splunk service):

```
docker exec -it webapp cat /opt/splunkforwarder/etc/system/local/outputs.conf
```

![](/images/15.png)

Opened app in browser. The webapp docker container was configured to listen for inbound connections on port 9999.

![](/images/9.png)

Logged back into the Splunk administration web console and peformed an analysis on the collected data events sent from the webapp docker container instance.

![](/images/11.png)

![](/images/12.png)
