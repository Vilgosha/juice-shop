pipeline {
    environment {
        registry = "vilgosha/debian"
        imageName = 'debian'
        registryCred = 'vilgosha'
        gitProject = "https://github.com/vilgosha/juice-shop.git"
    }
    agent any
    options {
        timeout(time: 1, unit: 'HOURS')
    }
    stages {
        stage ('preparation') {
            steps {
                deleteDir()
            }
        }
        stage ('get src from git') {
            steps {
                git 'https://github.com/vilgosha/juice-shop.git'
            }
        }
        stage ('run tests') {
            steps {
                echo 'npm run test'
            }
        }
        stage ('build docker') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage ('docker publish') {
            steps {
                script {
                    docker.withRegistry( '', registryCred ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage ('cleaning') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}