pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '192.168.1.197', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '192.168.1.104', description: 'Production Server')
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
                        sh "scp -i /home/student/.ssh/id_rsa **/target/*.war student@${params.tomcat_dev}:/var/lib/tomcat9/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/student/.ssh/id_rsa.pub **/target/*.war student@${params.tomcat_prod}:/var/lib/tomcat9/webapps"
                    }
                }
            }
        }
    }
}
