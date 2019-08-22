# Quarkus In A Box

This repo is related to a slide deck and a series of videos that will guide the user through a workshop to achieve the following:
* Gain an introductory knowledge of Quarkus -- Supersonic Subatomic Java
* Build and run a command-line application in Java using Quarkus
* Build, run and scale Quarkus containers on the OpenShift Kubernetes platform
* Build and deploy some Quarkus code to OpenShift from CodeReady Workspaces

## Prerequisites and Operating Environment

### **It is assumed that you have access to an OpenShift cluster.**

### The following must be installed or prepared:

1. Quarkus and all of it's related dependencies.
1. The OpenShift command line tool, `oc`.


This demo is operating system agnostic; you can use macOS, Linux or Windows with bash or PowerShell.

## Getting Started with Quarkus
This section will guide the user through the creation of a Quarkus application running from a terminal command line. This section *does not* use OpenShift.

### Create an app from nothing
### Running the Quarkus app in a JVM
### Building a standalone Quarkus app
### Running the super-fast Quarkus standalone app

## Quarkus in OpenShift with CodeReady Workspaces
### Create cluster
Create cluster
Log into dashboard as kubeadmin
Log into cluster as kubeadmin
`oc new-project quarkus`

### Install AMQ Streams from OperatorHub
#### Create instance of Kafka
Alter YAML to be only one replica, then click `Create`
Confirm that one pod is running
* AMQ Streams Operator will continuously watch for configuration changes and automatically apply them
Installed Operators -> link to operator -> All Instances: Select cluster; Change YAML to replicas: 3 and save it
Confirm that three pods are running

### Install Prometheus from OperatorHub

### Install Quarkus from OperatorHub
### Install CodeReady Workspaces from OperatorHub
#### Create Che Cluster

## Quarkus with CodeReady Workspaces
### Create Java + Quarkus + odo stack
#### Custom stack
#### Create Java + Quarkus + odo workspace
#### Import Quarkus project from github
#### Build and Run Locally
#### See results in browser


{
    "name": "Quarkus Java, CodeReady, odo",
    "description": "Java JDK Stack for Quarkus Apps",
    "scope": "general",
    "workspaceConfig": {
      "environments": {
        "default": {
          "recipe": {
            "type": "dockerimage",
            "content": "docker.io/schtool/che-quarkus-odo:latest"
          },
          "machines": {
            "dev-machine": {
              "env": {},
              "servers": {
                "8080/tcp": {
                  "attributes": {},
                  "protocol": "http",
                  "port": "8080"
                },
                "8000/tcp": {
                  "attributes": {},
                  "protocol": "http",
                  "port": "8000"
                },
                "5005/tcp": {
                  "attributes": {},
                  "protocol": "http",
                  "port": "5005"
                }
              },
              "volumes": {},
              "installers": [
                "org.eclipse.che.exec",
                "org.eclipse.che.terminal",
                "org.eclipse.che.ws-agent",
                "org.eclipse.che.ls.java"
  
              ],
              "attributes": {
                "memoryLimitBytes": "5368709120"
              }
            }
          }
        }
      },
      "commands": [
        {
          "commandLine": "mvn verify -f ${current.project.path}",
          "name": "Run Quarkus Tests",
          "type": "mvn",
          "attributes": {
            "goal": "Test",
            "previewUrl": ""
          }
        },
        {
          "commandLine": "mvn clean compile quarkus:dev -f ${current.project.path}",
          "name": "Start Live Coding",
          "type": "custom",
          "attributes": {
            "goal": "Run",
            "previewUrl": "${server.8080/tcp}"
          }
        },
        {
          "commandLine": "MAVEN_OPTS=\"-Xmx1024M -Xss128M -XX:MetaspaceSize=512M -XX:MaxMetaspaceSize=1024M -XX:+CMSClassUnloadingEnabled\" mvn -f ${current.project.path} clean package -Pnative -DskipTests",
          "name": "Build Native Quarkus App",
          "type": "custom",
          "attributes": {
            "goal": "Package",
            "previewUrl": ""
          }
        },
        {
          "commandLine": "MAVEN_OPTS=\"-Xmx1024M -Xss128M -XX:MetaspaceSize=512M -XX:MaxMetaspaceSize=1024M -XX:+CMSClassUnloadingEnabled\" mvn -f ${current.project.path} clean package -DskipTests",
          "name": "Create Executable JAR",
          "type": "custom",
          "attributes": {
            "goal": "Package",
            "previewUrl": ""
          }
        },
        {
          "commandLine": "mvn clean compile quarkus:dev -Ddebug -f ${current.project.path}",
          "name": "Debug Quarkus App",
          "type": "custom",
          "attributes": {
            "previewUrl": "${server.8080/tcp}",
            "goal": "Debug"
          }
        }
      ],
      "projects": [],
      "defaultEnv": "default",
      "name": "default",
      "links": []
    },
    "components": [
      {
        "version": "---",
        "name": "CentOS"
      },
      {
        "version": "1.8.0_45",
        "name": "JDK"
      },
      {
        "version": "3.6.0",
        "name": "Maven"
      },
      {
        "version": "2.4",
        "name": "Ansible"
      },
      {
        "version": "4.1.0",
        "name": "OpenShift CLI"
      }
    ],
    "creator": "ide",
    "tags": [
      "Java",
      "JDK",
      "Maven",
      "Ansible",
      "CentOS",
      "Git"
    ],
    "id": "quarkus-java"
  }
  