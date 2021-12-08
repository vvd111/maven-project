pipeline {
    agent any
    triggers {
     pollSCM('*/5 * * * *')
    }
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
         }
         stage ('Deployments'){
           parallel{
               stage('Deploy_to_staging'){
                steps {
                  sh "cp **/target/*.war /var/www/html "
                  }
                }
               stage ('Deploy to Production'){
                 steps{
                  timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                  }
                sh "cp **/target/*.war /var/www/html"
                }
             }
          }
        }
     }
