def tag = "foo"

pipeline {
    agent {
        label 'worker-linux'
    } 
    
    stages {

        stage('Compilacion') {
            steps {
               sh 'mvn -Dtag_name=value -DskipTests clean install package'
               echo '${tag}'
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
