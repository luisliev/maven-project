pipeline {
    agent any

    parameters {
        string(name: 'tomcat_dev', defaultValue: '100.27.23.86', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '54.145.88.106', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
    }

stages{
        stage('Build'){
            steps {
                sh 'export M2_HOME=/home/user/apache-maven-3.6.0'
                sh 'export M2=$M2_HOME/bin'
                sh 'export PATH=$M2:$PATH'
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
                stage ('deploy-to-staging'){
                    steps {
                        sh "scp **/target/*.war user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }

                stage ("deploy-to-prod"){
                    steps {
                        sh "scp **/target/*.war user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}