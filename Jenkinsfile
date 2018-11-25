pipeline {
    agent any

    options {
      buildDiscarder(logRotator(numToKeepStr: '30'))
    }

    stages {
        stage('Init') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                  echo 'Archiving...'
                  archiveArtifacts artifacts '**/target/*.war'
                }
            }
        }
    }
}
