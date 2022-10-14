pipeline{
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Pull Source Code from Github'){
            steps{
               git branch: 'main',
               credentialsId: 'Gitcred',
               url: 'https://github.com/anyaking922/ApplicationBuilder.git'
               
            }
        }
        stage('Code Analysis'){
            steps{
                withSonarQubeEnv('sonar') {
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
                sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/pet-adoption/target/spring-petclinic-2.4.2.war ec2-user@3.249.158.74:/opt/docker'
                }
             }
        }
    }
}
