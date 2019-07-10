pipeline {
    agent any

    parameters {
         string(name: 'tomcat_staging', defaultValue: '13.233.115.91', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '13.233.150.129', description: 'Production Server')
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
                        bat "cp -i C:/Users/z022140/Google Drive/Personal/ec2tutorial.pem **/target/*.war ec2-user@${params.tomcat_staging}:/etc/tomcat8/apache-tomcat-8.5.42/webapps" 
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "cp -i C:/Users/z022140/Google Drive/Personal/ec2tutorial.pem **/target/*.war ubuntu@${params.tomcat_prod}:/etc/tomcat8/apache-tomcat-8.5.42/webapps"
                    }
                }
            }
        }
    }
}
