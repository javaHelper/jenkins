# jenkins

# install/Setup jenkins

```sh
docker run -p 8080:8080 -p 50000:50000 --restart=on-failure jenkins/jenkins:lts-jdk17
```

```sh
docker container ls
CONTAINER ID   IMAGE                           COMMAND                  CREATED          STATUS          PORTS                                              NAMES
c19729f2e4e1   jenkins/jenkins:lts-jdk17       "/usr/bin/tini -- /u…"   15 minutes ago   Up 14 minutes   0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   elastic_hawking
c259ca9f2684   mysql:8.0.32                    "docker-entrypoint.s…"   7 days ago       Up 2 days       0.0.0.0:3306->3306/tcp, 33060/tcp                  hibernatespringbootcalculatepropertygenerated-mysql-1
37b36ddb9999   moby/buildkit:buildx-stable-1   "buildkitd"              7 days ago       Up 7 days                                                          buildx_buildkit_determined_matsumoto0
```

```sh
docker exec -it -u root c19729f2e4e1 /bin/bash
```

```sh
root@c19729f2e4e1:/# apt-get update 
root@c19729f2e4e1:/# apt-get install maven
```

# 

```
pipeline {
    agent any
    tools {
        maven 'Maven 3.9.2' // Match the name in Global Tool Configuration
    }
    stages {
        stage("Clean Up") {
            steps {
                deleteDir()
            }
        }
        stage("Clone Repo") {
            steps {
                sh "git clone https://github.com/jenkins-docs/simple-java-maven-app.git"
            }
        }
        stage("Build") {  // <-- Missing curly brace added
            steps {
                dir("simple-java-maven-app") {
                    sh "mvn clean install"
                }
            }  // <-- Properly closed steps block
        }
        stage("Test") {  // <-- Missing curly brace added
            steps {
                dir("simple-java-maven-app") {
                    sh "mvn test"
                }
            }  // <-- Properly closed steps block
        }
    }
}
```

