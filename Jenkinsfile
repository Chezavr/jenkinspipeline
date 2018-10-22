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
                        sh "rm /Users/chezavr/Downloads/apache-tomcat-8.5.34-staging/webapps/*.war"
                        sh "cp -i /Users/chezavr/.jenkins/workspace/fully_automated/webapp/target/*.war /Users/chezavr/Downloads/apache-tomcat-8.5.34-staging/webapps"
                    }
                }

                stage ("Deploy to Production") {
                    steps {
                        sh "rm /Users/chezavr/Downloads/apache-tomcat-8.5.34-prod/webapps/*.war"
                        sh "cp -i /Users/chezavr/.jenkins/workspace/fully_automated/webapp/target/*.war /Users/chezavr/Downloads/apache-tomcat-8.5.34-prod/webapps"
                    }
                }
            }
        }
    }
}