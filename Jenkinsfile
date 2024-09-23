pipeline {
    agent any
    stages {
        stage("Checkout") {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/thumarkrisha/Docker-pipeline']])
            }
        }
        stage("Build Image") {
            steps {
                script {
                    if (isUnix()) {
                        sh 'docker build -t krisha2308/dockerpipeline .'
                    } else {
                        bat 'docker build -t krisha2308/dockerpipeline .'
                    }
                }
            }
        }
        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                    script {
                        if (isUnix()) {
                            sh "docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD"
                            sh 'docker push krisha2308/dockerpipeline'
                            sh 'docker logout'
                        } else {
                            bat "docker login -u %DOCKERHUB_USERNAME% -p %DOCKERHUB_PASSWORD%"
                            bat 'docker push krisha2308/dockerpipeline'
                            bat 'docker logout'
                        }
                    }
                }
            }
        }
        stage('Cleanup') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'docker rmi krisha2308/dockerpipeline || true'
                    } else {
                        bat 'docker rmi krisha2308/dockerpipeline || exit 0'
                    }
                }
            }
        }
    }
}
