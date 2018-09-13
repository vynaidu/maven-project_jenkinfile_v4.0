pipeline {
    agent any

    parameters {
         string(name: 'tomcat-staging', defaultValue: '54.200.171.161', description: 'Staging Server')
         string(name: 'tomcat-prod', defaultValue: '54.245.5.222', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
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

        
    }
}