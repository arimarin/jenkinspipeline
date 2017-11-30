pipeline {
    agent any

    parameters {
        string('tomcat_dev', defaultValue: 'localhost:8888', description: 'Staging Server')
        string('tomcat_prod', defaultValue: 'localhost:8889', description: 'Production Server')
    }
    
    triggers {
        // Poll Git every minute
        pollSCM('* * * * *')
    }

    stages {
        stage('Build') {
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
        stage ('Deployments') {
            parallel {
                stage('Deploy to Staging') {
                    steps {
                        sh "cp -f **/target/*.war /usr/local/apache-tomcat-8.5.23/webapps"
                        //sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage('Deploy to Production') {
                    steps {
                        sh "cp -f **/target/*.war /usr/local/apache-tomcat-8.5.23-2/webapps"
                        //sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}