pipeline {
    agent any

    tools {
        jdk 'OpenJDK8'
        maven 'Maven3'
    }

    stages {
        stage('Initialize') {
            steps {
                script {
                    def dockerHome = tool 'Docker'
                    env.PATH = "${dockerHome}/bin:${env.PATH}"
                }
            }
        }

        stage('SCM') {
            steps {
                git changelog: false, poll: false, url: 'https://github.com/SumitKumar121999/tastlast.git'
            }
        }

        stage('Maven Build') {
            steps {
                sh "mvn clean install"
            }
        }

        stage('Docker Build and Push') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                   sh '''
                       docker login -u sksumit1999 -p $dockerhubpwd
                       docker build -t sksumit1999/dockertask:tag1234 .
                       docker push sksumit1999/dockertask:tag1234
                     '''

                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
