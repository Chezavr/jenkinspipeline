pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                // sh 'mvn clean package'
                build job 'package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to Staging!:D') {
            steps {
                build job: 'Deploy-to-staging'
            }
        }
        stage ('Deploy to Production') {
            steps {
                timeout (time: 5, unit: 'DAYS') {
                    input message: 'Approve PRODUCTION Deployment?'
                }

                build job: 'Deploy-to-Prod'
            }
            post {
                success {
                    echo 'Code deployed to Production successfully!'
                }

                failure {
                    echo 'Code wasn\'t deployed to Production successfully'
                }
            }
        }
    }
}