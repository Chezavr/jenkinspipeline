pipeline {
    agent any
    
    parameters {
        string(name: 'tomcat_dev', defaultValue: 'http://localhost:8090', description: 'Staging Server')
        string(name: 'tomcat_dev', defaultValue: 'http://localhost:9090', description: 'Production Server')
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments') {
            parallel {
                stage ('Deploy to Staging') {
                    steps {
                        sh "cp -i **/target/*.war /Users/chezavr/Downloads/apache-tomcat-8.5.34-prod/webapps"
                    }
                }

                stage ("Deploy to Production") {
                    steps {
                        sh "cp -i **/target/*.war /Users/chezavr/Downloads/apache-tomcat-8.5.34-prod/webapps"
                    }
                }
            }
        }
    }
    // stages{
    //     stage('Build'){
    //         steps {
    //             // sh 'mvn clean package'
    //             build job: 'package'
    //         }
    //         post {
    //             success {
    //                 echo 'Now Archiving...'
    //                 archiveArtifacts artifacts: '**/target/*.war'
    //             }
    //         }
    //     }
    //     stage ('Deploy to Staging!:D') {
    //         steps {
    //             build job: 'Deploy-to-staging'
    //         }
    //     }
    //     stage ('Deploy to Production') {
    //         steps {
    //             timeout (time: 5, unit: 'DAYS') {
    //                 input message: 'Approve PRODUCTION Deployment?'
    //             }

    //             build job: 'Deploy-to-Prod'
    //         }
    //         post {
    //             success {
    //                 echo 'Code deployed to Production successfully!'
    //             }

    //             failure {
    //                 echo 'Code wasn\'t deployed to Production successfully'
    //             }
    //         }
    //     }
    // }
}