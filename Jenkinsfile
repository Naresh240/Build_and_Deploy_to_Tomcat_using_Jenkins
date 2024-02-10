pipeline {
    agent any

    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "54.211.147.160:8081"
        NEXUS_REPOSITORY = "54.211.147.160:8081"
        NEXUS_CREDENTIAL_ID = "nexus-creds"
    }

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
        stage("Code_Coverage") {
            steps {
                withSonarQubeEnv(installationName: 'sonarqube', credentialsId: 'sonar-token') {
                    sh "${ tool ("sonar-scanner")}/sonar-scanner -Dsonar.projectKey=hellospringboot -Dsonar.projectName=hellospringboot -Dsonar.sourceEncoding=UTF-8 -Dsonar.sources=src"
                }
            }
        }
        stage("Deploy_Artifact_To_Nexus") {
            steps {
                script{
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "* File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "* File: ${artifactPath}, could not be found";
                    }
                }
            }
        }        
    }
}