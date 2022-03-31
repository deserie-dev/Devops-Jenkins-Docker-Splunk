# Jenkins CICD Pipeline to Build a Docker Image with Splunk Integration

Description
Integrating Jenkins with Docker provides a robust CICD method for building and packaging runnable containers. Splunk can be used to monitor and assess their operational performance, through its support for enterprise-grade monitoring, logging, and diagnostics collection.

In this lab, I launched a Jenkins and Splunk CICD and monitoring environment using Docker containers on a provided EC2 instance. I then configured a Jenkins build pipeline to build, compile, and package a sample Java servlet web application into a runnable Docker image. The Jenkins build process creates the Docker image using a Dockerfile. The build process integrates Splunk logging into the Docker image, by installing the Splunk Forwarding Agent at build time. At runtime, the docker container will then have the ability to publish runtime logging information into the Splunk service.

![](/images/1.png)

---
