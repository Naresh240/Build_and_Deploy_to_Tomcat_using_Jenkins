pipeline {
    agent any

    stages {
        stage("Checkout") {
            steps {
                git branch: 'master', 
                    url: 'https://github.com/Naresh240/Build_and_Deploy_to_Tomcat_using_Jenkins.git'
            }
        }
        stage("Build_Artifact") {
            steps {
                sh "mvn clean package"
            }
        }
        stage("Code Coverage") {
            steps {
                withSonarQubeEnv(installationName: 'sonarqube', credentialsId: 'sonar-token') {
                    sh "${ tool ("sonar-scanner")}/sonar-scanner -Dsonar.projectKey=hellospringboot -Dsonar.projectName=hellospringboot -Dsonar.sourceEncoding=UTF-8 -Dsonar.sources=src"
                }
            }
        }
    }
}