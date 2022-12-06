pipeline {

    agent {
        label 'worker-linux'
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

        stage('Procesos docker - Build Tag y Push') {
            steps {
                sh 'mvn compile jib:build'
            }
        }

        stage('Iniciando QA') {

            steps {
                script {
                    def props
                    props = readProperties file: 'versions/version.properties'

                    echo "My tag is ${props['version']}"

                    build job: 'multibranch-project-qa', parameters: [string(name: 'TAG_VERSION', value: "${props['version']}")]
                }
            }
        }

    }
}