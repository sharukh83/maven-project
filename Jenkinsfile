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
    
}
}


