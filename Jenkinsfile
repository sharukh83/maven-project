pipeline {
    agent any
    stages {
        stage('Build') {
            steps{
                sh 'mvn clean package'
            }
            post {
                success { 
                    echo 'Now Archiving'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Build to stage') {
            steps{
                build job: 'deploy-staging'
            }
        }

        stage('Build to production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message: 'Approve production deployment?'
                }

                build job: 'deploy-to-prod'

            }
            post {
                success {
                   echo 'Code deplo to production'
                }
                failure{
                   echo  'Deployment failed'
                }
            }
                 
            }
        
    
}
}


