pipeline {
    agent any

    parameters {
         string(name: 'tomcat_staging', defaultValue: '13.233.115.91', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '13.233.150.129', description: 'Production Server')
		 string(name: 'ppk_path', defaultValue: 'C:/Users/z022140/Google Drive/Personal/ec2tutorial.pem', description: 'full path to ppk file')
         string(name: 'war_path', defaultValue: 'C:/Program Files (x86)/Jenkins/workspace/FullyAutomated/webapp/target/*.war', description: 'full path to war file')
         string(name: 'target_path', defaultValue:'/etc/tomcat8/apache-tomcat-8.5.42/webapps', description: 'full path in the target server')
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
						bat "PATH=/sbin:/usr/sbin:/usr/bin:/usr/local/bin pscp -i ${params.ppk_path} ${params.war_path} ec2-user@${params.tomcat_staging}:${target_path}"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "PATH=/sbin:/usr/sbin:/usr/bin:/usr/local/bin pscp -i ${params.ppk_path} ${params.war_path} ec2-user@${params.tomcat_prod}:${target_path}"
                    }
                }
            }
        }
    }
}
