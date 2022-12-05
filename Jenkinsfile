pipeline {

    agent {
        label 'worker-linux'
    }

    environment {
        //Use Pipeline Utility Steps plugin to read information from pom.xml into env variables
        VERSION_NUMBER = readMavenPom().getVersion()

        }
    
    stages {

        stage('Compilacion') {
            steps {
               sh 'mvn -Dtag=value -DskipTests clean install package'
               echo 'My tag is ${VERSION_NUMBER}'
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
