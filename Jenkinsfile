pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '3.141.170.47', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '3.142.244.46', description: 'Production Server')
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
                        sh "scp -i /home/student/Downloads/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/opt/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/student/Downloads/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/opt/tomcat8/webapps"
                    }
                }
            }
        }
    }
}
