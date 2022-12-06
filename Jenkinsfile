
def getProps() {
    node {
        def props = readProperties file: '/multibranch-project/version.properties'
        return props
    }
}

pipeline {

    agent {
        label 'worker-linux'
    }

    stages {
        stage('Compilacion') {

            steps {
               def props =  getProps()
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