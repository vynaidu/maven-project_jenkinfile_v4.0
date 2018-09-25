pipeline {
    agent any

    parameters {
         string(name: 'tomcat_staging', defaultValue: '35.167.159.177', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '34.212.45.8', description: 'Production Server')
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/ubuntu/tomcat-ubuntu.pem /var/lib/jenkins/workspace/first_maven_project/webapp/target/*.war ssh ubuntu@${params.tomcat_staging}:/var/lib/tomcat87/webapps" 
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/ubuntu/tomcat-ubuntu.pem /var/lib/jenkins/workspace/first_maven_project/webapp/target/*.war ssh ubuntu@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}
