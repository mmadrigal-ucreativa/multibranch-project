pipeline {

    agent {
        label 'worker-linux'
    }

    stages {
        stage('Compilacion') {
            steps {
               def props
               props = readProperties file: '/multibranch-project/versions/version.properties'
               sh 'mvn -DskipTests clean install package'
               echo "My tag is ${props['version']}"
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