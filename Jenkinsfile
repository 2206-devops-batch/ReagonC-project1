def img
pipeline {
    environment {
        registry = "chanreagon/python-jenkins"
        registryCredential = 'docker-hub-login'
        dockerImage = ''
    }
    agent any

    stages {

        stage('build checkout') {
            steps {
                git 'https://github.com/2206-devops-batch/ReagonC-project1.git'
            }
        }

        stage('build image') {
            steps {
                script {
                    img = registry + ":${env.BUILD_ID}"
                    println ("${img}")
                    dockerImage = docker.build("${img}")
                }
            }
        } 

        stage('Testing - running in Jenkins Node') {
            steps {
                sh "docker run -d --name ${JOB_NAME} -p 5000:5000 ${img}"
            }
        }

        stage("push to DockerHub") {
            steps {
                script {
                    docker.withRegistry('http://registry.hub.docker.com', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}