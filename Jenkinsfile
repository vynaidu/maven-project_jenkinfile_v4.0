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
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: 'C:/Program Files (x86)/Jenkins/workspace/FullyAutomated_aws/webapp/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i tomcat-test.pem C:/Program Files (x86)/Jenkins/workspace/FullyAutomated_aws/webapp/target/*.war ec2-user@${params.tomcat_staging}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i tomcat-test.pem C:/Program Files (x86)/Jenkins/workspace/FullyAutomated_aws/webapp/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}