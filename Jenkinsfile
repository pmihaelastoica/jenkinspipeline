pipeline {
    agent any
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
