pipeline {
    agent any
    tools {
        maven 'M2_HOME'
        jdk 'localJDK'
    }
    parameters {
         string(name: 'tomcat_dev', defaultValue: '99.79.16.236', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.60.170.131', description: 'Production Server')
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
                stage ('Deploy to China'){
                    steps {                    
                        sh "scp -r -i /var/lib/jenkins/workspace/FullyAutomated/webapp/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Germany"){
                    steps {
                        sh "scp -r -i /var/lib/jenkins/workspace/FullyAutomated/webapp/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}