
pipeline {

    agent {
        label 'worker-linux'
    }

    def props = readProperties defaults: d, file: '/multibranch-project/logs/log-ant-run.properties'

    environment {

        //Use Pipeline Utility Steps plugin to read information from pom.xml into env variables
        TAG_VERSION = props['tag.version']
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
