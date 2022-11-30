pipeline {
    agent {
        label 'linux-worker'
    }

    stages {

        stage('Compilacion') {
            steps {
               sh 'mvn -DskipTests clean install package'
            }
        }

        stage('Unit test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                   junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Build docker image') {
            steps {
                sh 'docker image build -t spring-webapp .'
            }
        }

        stage('Upload docker image') {
            steps {
                withCredentials([string(credentialsId: 'docker-credentials', variable: 'docker-credentials')]) {
                    sh 'docker login -u mmadrigal -p ${docker-credentials}'
                    sh 'docker image push mmadrigal/spring-webapp:latest'
                }
            }
        }
    }
}
