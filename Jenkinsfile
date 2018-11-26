// a pipline to deploy on ec2 tomcat containers
pipeline {
    agent {
        node { label 'sony-host' }
    }
    
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    
    parameters {
        string(name:'tomcat_dev', defaultValue:'18.195.169.107', description:'dev ec2 instance')
        string(name:'tomcat_prod', defaultValue:'3.120.139.218', description:'dev ec2 instance')
    }
    
    triggers {
        pollSCM('* * * * *')
    }
    
    stages {
        stage('Init'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving...'
                    archiveArtifacts '**/target/*.war'
                }
            }
        }
        stage ('deploy to aws'){
            parallel {
                stage('Deploy to staging'){    
                    steps {
                        sh "scp -i /home/alexst/tomcat.pem **/target/*.war ec2-user@${param.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
                stage('Deploy to production'){    
                    steps {
                        sh "scp -i /home/alexst/tomcat.pem **/target/*.war ec2-user@${param.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}



/*
// a pipline to deploy on local tomcat containers
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
            }
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
*/
