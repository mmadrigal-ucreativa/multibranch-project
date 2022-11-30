pipeline {
    agent {
        label 'linux-worker'
    }

    stages {

        stage('Compilacion') {
            steps {
               sh 'mvn -DskipTests clean install package'
            }
        }
    }
}
