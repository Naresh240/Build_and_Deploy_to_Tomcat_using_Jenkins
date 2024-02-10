pipeline {
    agent any

    stages {
        stage("Checkout") {
            steps {
                git branch: 'master', 
                    url: 'https://github.com/Naresh240/Build_and_Deploy_to_Tomcat_using_Jenkins.git'
            }
        }
    }
}