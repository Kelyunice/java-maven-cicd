# MAVEN BUILD TOOL 
# maven is written in xml so Maven uses xml but Gradle uses Groovy language both tools have command line tools and commands to execute the tasks

for java code we have Maven and Gradle
This build tools install the dependencies and 
compile and compress code to reduce the seize

BUILDING ARTIFACT
## To build an artifact we need to install the following in the virtual machine or server:

## 1. Update the apt repo: 
```
sudo apt update
```
## 2. Install java: 
```
sudo apt install java
```

```
sudo apt install git
```
## 3. install Maven from the software downloaded from the internet
```
 wget https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz
```
## 4. untar the file

```
 tar xzvf apache-maven-3.9.9-bin.tar.gz
```
## 5.validate the project structure and resource files

```
mvn validate
```
## 6. it will compile all java classes and test classes
```
mvn compile
```

## 7. it will run the unit test cases (JUNit)

```
mvn test
```

## 8. package the application code into an artifact this will create a target directory 

```
mvn package
```

## 9. l store the build artifacts in the MAVEN LOCAL REPOSITORY (.m2/repository)

```
mvn install
```


 mvn package = will build the application code into Artifact
 mvn deploy  = publish the artifact in the artifactory repository like Nexus

 ## The pom.xml file is where all the java dependencies are listed or manage for Maven

 The build.gradle file is where all the gradle dependecies are listed and manage

 All this dependencies come from the repositories like Maven central Repositories(MVNrepository) which is used to find the dependencies and their versions and download from remote repository to local repository.
For example if we want to connect our java application to ElasticSearch we need a library(dependency) for the connection so what do we do:
1. We need to find a dependency with name and version
2. we need to add the dependency to the dependencies file (pom.xml) to install the dependencies
3. Dependency will gets downloaded locally to maven local repository (when we run mvn package it download the dependecy of the project and it proceed to build the applicaton) 

the dependencies locally are found in the home folder in .m2/repository

what is the pre-requisite to build an artifact in maven we need to do confuguration in the pom.xml file and the setting.xml file

> once the artifact have been build you can run the application with the command === java -jar <Name of jar file>

```
java -jar target/java-maven-app-1.0-snapshot.jar
```

What is an artifacts: it is the application built into a single file the artifact can have different format(jar,war,zip,tar file)



once the artifact have been build you can push to an artifact repository so that it is kept as a backup incase we want to reuse it example of artifactory repository are
- Jfrog Artifactory
- Nexus Artifactory

The type of Artifact will depend on the programming language:
If the programming language is Java the artifact will be JAR or WAR file
JAR = Java Archive (include the source code and the dependencies use by the code to run like Spring framework, datetime libraries, pdf processing libraries) 

You can fetch or download your artifact from that repository using (curl or wget) on the operating system server that you are working on

We will not publish and fetch the artifact like that because of Docker and cicd pipeline
***Building and distributing artifact is possible because of docker so with docker it permit us to walk with just one type of artifact which is Docker image instead of having different type of artifact like Jar and War artifact. so we just need to build Docker Images from this application instead of Jar, War or Zip artifact.

SO we may not need a repository that support different artifact type we just need a repository that suport Docker Images
Docker Image is an artifact so it is an alternative to all the different artifact type 

so we have to copy the Jar file into the docker image and then build the application

you are responsible for configuring the building of the artifact which should be automated using the cicd pipeline

## ARTEFACT REPOSITORY MANGER
## NEXUS IS a repository manager that allow you to upload and store different build artifacts and to retrieve them when ever you need them (internally used privately, in some companies)
Public repository managers like maven central repository
