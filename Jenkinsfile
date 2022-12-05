pipeline {

    agent {
        label 'worker-linux'
    }

    stages {

        stage('Compilacion') {
            steps {
               sh 'mvn -DskipTests clean install package'


               script {
                    def tag_version = sh script: 'mvn help:evaluate -Dexpression=version.number -q -DforceStdout', returnStdout: true
                    echo "My tag is ${tag_version}"
               }


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

        stage('Procesos docker - Build Tag y Push') {
            steps {
                sh 'mvn compile jib:build'
            }
        }
        

    }
}
