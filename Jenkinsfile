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
                // sh 'export PATH=/home/user/apache-maven-3.6.0/bin:$PATH'
                sh '/home/user/apache-maven-3.6.0/bin/mvn clean'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.jar'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('deploy-to-staging'){
                    steps {
                        sh "scp **/target/*.jar user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }

                stage ("deploy-to-prod"){
                    steps {
                        sh "scp **/target/*.jar user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}