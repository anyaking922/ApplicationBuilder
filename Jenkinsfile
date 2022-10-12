pipeline{
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Pull Source Code from Github'){
            steps{
               git branch: 'main',
               credentialsId: 'f641d140-c071-4b64-9334-5c616f8c47fa',
               url: 'https://github.com/anyaking922/ApplicationBuilder.git'
               
            }
        }
        stage('Code Analysis'){
            steps{
                withSonarQubeEnv('sonarQube') {
                    sh "mvn sonar:sonar"
                }
            }
        }
        stage('Building Source Code') {
            steps{
               sh 'mvn package -Dmaven.test.skip'
            }
        }
        stage('Sending Artifacts') {
            steps{
                sshagent(['jenkinskey']) {
                sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/petadoption/target/spring-petclinic-2.4.2.war ec2-user@54.229.122.183:/opt/docker'
                }
             }
        }
    }
}
