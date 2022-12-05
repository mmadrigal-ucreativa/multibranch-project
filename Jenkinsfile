def tag_name = "foo"

pipeline {
    agent {
        label 'worker-linux'
    } 
    
    stages {

        stage('Compilacion') {
            steps {
               sh 'mvn -Dtag_name=value -DskipTests clean install package'
            }
        }

        stage('Paso de prueba') {
            steps {
               echo '${tag_name}'
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
