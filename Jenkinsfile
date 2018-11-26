pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }

    stages {
        stage('Init'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to staging'){    
            steps {
                build job: 'deploy-to-staging'
            }
        }
        stage('Deploy to production'){
            steps {
                timeout(time:5, unit:'DAYS'){
                    input message: 'Aprove PRODUCTION deployment'
                }
                build job: 'deploy-to-prod'
            post {
                success {
                    echo 'Successfully deployed'
                }
                failure {
                    echo 'Build failed'
                }
            }
        }
    }
}
