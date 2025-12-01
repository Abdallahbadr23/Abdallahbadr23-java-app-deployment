pipeline {
    agent { label 'built-in' }

    tools {
        maven 'maven-3.9.11'
        jdk 'java-11'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Hassan-Eid-Hassan/cicd-lab2.git'
            }
        }

        stage('Build App') {
            steps {
                sh 'mvn package -DskipTests'
                sh 'java --version'
            }
        }

        stage('arti fact') {
            steps {
                archiveArtifacts artifacts: '**/*.jar'
            }
        }

        stage('docker build') {
            steps {
                sh "docker build -t abdallah279/java-app:v$BUILD_NUMBER ."
            }
        }

        stage('docker push') {
            steps {
                withCredentials([string(credentialsId: 'username', variable: 'docker_username'), string(credentialsId: 'password', variable: 'docker_password')]) {
                     sh """
                        docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}
                        docker push abdallah279/java-app:v$BUILD_NUMBER
                    """
                    }
               
            }
        }
       stage('deploy application') {
            steps {
                sh "docker run -d -p 8090:8090 --name java-app-$BUILD_NUMBER  abdallah279/java-app:v$BUILD_NUMBER  "
            }
        } 
    }
}
