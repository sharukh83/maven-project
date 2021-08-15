pipeline {
    agent any
    
    parameters {
        string(name:'tomcat_dev', defaultValue: '18.222.150.172', description: 'dev environment')
        string(name: 'tomcat_prod', defaultValue: '3.144.36.147', description: 'prod environment')
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
                echo 'NoW Archiving'
                archiveArtifacts artifacts: '**/target/*.war'
            }
        }
    }

    stage('Deployment'){
        parallel{
            stage ('Deploy to staging'){
                steps {
                    sh "scp -i /home/ubuntu/var/lib/jenkins/amerkey.pem **/target/*.war ubuntu@${params.tomcat_dev}:/var/lib/tomcat9/webapps"
                }
            }

            stage ("Deploy to production"){
                steps {
                     sh "scp -i /home/ubuntu/var/lib/jenkins/amerkey.pem **/target/*.war ubuntu@${params.tomcat_prod}:/var/lib/tomcat9/webapps"
                }
            }
        }
    }
}
}


