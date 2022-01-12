pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '192.168.56.121', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '192.168.56.121', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
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
                        sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ubuntu@${params.tomcat_dev}:/home/ubuntu/apache-tomcat-8.5.73/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ubuntu@${params.tomcat_prod}:/home/ubuntu/apache-tomcat-8.5.73/webapps"
                    }
                }
            }
        }
    }
}
