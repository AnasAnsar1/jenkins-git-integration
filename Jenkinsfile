pipeline {
    agent any
    stages{
        stage('SCM'){
            steps {
                git branch: 'dev', url: 'https://github.com/AnasAnsar1/jenkins-git-integration.git'
            }
        }
        stage("build & SonarQube analysis") {
            steps {
                withSonarQubeEnv('SONAR_FEB23') {
                sh 'mvn clean package sonar:sonar'
            }
            }
    }
    }
}