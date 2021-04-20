pipeline{
    environment {
        registry = "https://hub.docker.com/u/vilgosha"
        imageName = 'debian'
        registryCred = 'vilgosha'
        gitProject = "https://github.com/Vilgosha/juice-shop.git"
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
                git url: "${gitProject}"
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
                    //hub.docker.com/u/vilgosha + / + my-juicy-shop = vilgohsa/my-juicy-shop
                    dockerImage = docker.build("${registry}/${imageName}")
                }
            }
        }
        stage ('docker publish') {
            steps {
                script {
                    docker.withRegistry("${registry}${imageName}", "${registryCred}") {
                        dockerImage.push()
                        dockerImage.push("${version}")
                    }
                }
            }
        }
        stage ('cleaning') {
            steps {
                sh 'docker rmi -f ${docker images -a -q}'
            }
        }
    }
}