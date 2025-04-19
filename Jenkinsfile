pipeline {
    agent { 
        docker {
            image 'maven:3-alpine'
    
    tools {
        jdk 'jdk17'
        maven 'maven3'
    }
    
    environment {
        SCANNER_HOME= tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git credentialsId: 'git-cred2', url: 'https://github.com/Kelyunice/java-maven-cicd.git'
            }
        }

        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }
        stage('Test') {
            steps {
                sh "mvn test"
            }
        }
        stage('File System Scan') {
            steps {
                sh "trivy fs ."
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=java-maven-cicd -Dsonar.projectKey=java-maven-cicd \
                           -Dsonar.java.banaries=. '''     
                }
            }
        }
        stage('Quality Gate') {
            steps {
                script {
                  waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
        stage('Build Artifact') {
            steps {
                sh "mvn package"
            }
        }
        stage('Publish To Nexus') {
            steps {
               withMaven(globalMavenSettingsConfig: 'global-settings', jdk: 'jdk17', maven: 'maven3', mavenSettingsConfig: '', traceability: true) {
                        sh "mvn deploy"
               }
            }
        }
        stage('Build & Tag Docker Image') {
            steps {
                script {
                   withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                            sh "docker build -t kelyunice1419/java-maven-app:2.0 ."
                   }    
                }     
            } 
        }
        stage('Docker Scan Image') {
            steps {
                sh "trivy image kelyunice1419/java-maven-app:2.0"
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred2', toolName: 'docker') {
                    sh "docker push kelyunice1419/java-maven-app:2.0"
                    }
                } 
            }
        }
        stage('Deploy To Kubernetess') {
            steps {
               withKubeConfig(caCertificate: '', clusterName: ' kubernetes', contextName: '', credentialsId: 'k8-cred', namespace: 'webapp', restrictKubeConfigAccess: false, serverUrl: 'https://172.31.16.14:6443') {
                        sh "kubectl apply -f deployment.yaml"
               } 
            }
        }
        
        stage('Verify the Deployment') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: ' kubernetes', contextName: '', credentialsId: 'k8-cred', namespace: 'webapp', restrictKubeConfigAccess: false, serverUrl: 'https://172.31.16.14:6443') {
                         sh "kubectl get pods -n webapp"
                         sh "kubectl get svc -n webapp"
                   }
                 }  
             }
        }
     }
 }   
}
