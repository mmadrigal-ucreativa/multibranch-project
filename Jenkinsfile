pipeline {
def tag = "foo"
    agent {
        label 'worker-linux'
    } 
    
    stages {

        stage('Compilacion') {
            steps {
               sh 'mvn -Dtag=value -DskipTests clean install package'
               echo 'My tag is ${tag}'
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
