pipeline {
    agent {
        label 'linux-worker'
    }

    stages {
        stage('Compilacion') {
            sh 'mvn -DskipTests clean install package'
        }
    }
}
