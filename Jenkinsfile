pipeline {
    agent any

    parameters {
         string(name: 'tomcat_staging', defaultValue: '13.232.168.168', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.66.214.0', description: 'Production Server')
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/ubuntu/tomcat-stage2.ppk /var/lib/jenkins/workspace/FullyAutomated/webapp/target/*.war ubuntu@${params.tomcat_staging}:/var/lib/tomcat8/webapps" 
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/ubuntu/tomcat-prod.ppk /var/lib/jenkins/workspace/FullyAutomated/webapp/target/*.war ubuntu@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}
