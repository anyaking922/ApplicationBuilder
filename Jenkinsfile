pipeline{
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Pull Source Code from Github'){
            steps{
               git branch: 'main',
               credentialsId: '86a0acf2-edf7-4a1d-bc4c-274587649a9e',
               url: 'https://github.com/Daicon001/Application.git'
               
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
                sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/petadoption/target/spring-petclinic-2.4.2.war ec2-user@35.85.151.175:/opt/docker'
                }
             }
        }
    }
}
