def getProps() { return {
        node {
            def props = readProperties file: '/multibranch-project/versions/version.properties'
        }
    }
}

pipeline {

    agent {
        label 'worker-linux'
    }

    stages {
        stage('Compilacion') {
            def props = getProps()
            steps {
                echo "${props}"
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