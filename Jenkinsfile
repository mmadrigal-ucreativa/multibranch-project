pipeline {

    agent {
        label 'worker-linux'
    }

    environment {
        //Use Pipeline Utility Steps plugin to read information from pom.xml into env variables
        TAG_VERSION = readMavenPom().version.number

        }
    
    stages {

        stage('Compilacion') {
            steps {
               sh 'mvn -DskipTests clean install package'
               echo "My tag is ${TAG_VERSION}"
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
