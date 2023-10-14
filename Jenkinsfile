pipeline {
    agent any
    
    tools {
        maven 'maven'
    }
    parameters {
         string(name: 'staging_server', defaultValue: '54.236.7.198', description: 'Remote Staging Server')
    }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ("Deploy to Staging"){
                    steps {
                        sh 'scp Dockerfile ubuntu@54.236.7.198'
                        sh 'ssh ubuntu@54.236.7.198 "docker build . -t tomcatwebapp:${env.BUILD_ID}"'
                    }
                }
            }
        }
    }
}
