pipeline {
    agent any

    parameters {
        string(name: 'tomcat_dev', defaultValue: '52.91.211.129', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '100.25.221.126', description: 'Production Server')
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
                stage ('deploy-to-staging'){
                    steps {
                        bat "winscp **/target/*.war user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }

                stage ("deploy-to-prod"){
                    steps {
                        bat "winscp **/target/*.war user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}