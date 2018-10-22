pipeline {
    agent any
    
    parameters {
        string(name: 'tomcat_stage', defaultValue: '/Users/chezavr/Downloads/apache-tomcat-8.5.34-staging', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '/Users/chezavr/Downloads/apache-tomcat-8.5.34-prod', description: 'Production Server')
        string(name: 'workspace_path', defaultValue: '/Users/chezavr/.jenkins/workspace/', description: 'Path to the workspace')
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
                        sh "rm ${params.tomcat_stage}/webapps/*.war" 
                        sh "cp -i /Users/chezavr/.jenkins/workspace/fully_automated/webapp/target/*.war ${params.tomcat_stage}/webapps"
                    }
                }

                stage ("Deploy to Production") {
                    steps {
                        sh "rm ${params.tomcat_prod}/webapps/*.war"
                        sh "cp -i /Users/chezavr/.jenkins/workspace/fully_automated/webapp/target/*.war ${params.tomcat_prod}/webapps"
                    }
                }
            }
        }
    }
}