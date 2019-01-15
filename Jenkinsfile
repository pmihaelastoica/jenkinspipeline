pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: 'http://localhost:8090/', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'http://localhost:8081/', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        build job: 'Deploy to Staging'
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        timeout(time:5, unit:'DAYS'){
                            input message: 'Approve PRODUCTION Deployment?'
                    }
                        
                        build job: 'DEPLOY TO PROD'
                }
                    post {
                        success {
                            echo 'Code deployed to Production.'
                        }
                        
                        failure {
                            echo 'Deployment failed'
                        }
            }
        }
    }
}
