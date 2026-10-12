```groovy
pipeline {
    agent any

    tools {
        maven "MVN_HOME"
    }

    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "3.133.145.136:8081"
        NEXUS_REPOSITORY = "devops"
        NEXUS_CREDENTIAL_ID = "Nexus_server"
    }

    stages {
        stage("clone code") {
            steps {
                git 'https://github.com/satishcloud00/spring3-mvc-maven-xml-hello-world-1.git'
            }
        }

        stage("mvn build") {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true clean install'
            }
        }

        stage("publish to nexus") {
            steps {
                script {
                    def pom = readMavenPom file: "pom.xml"
                    def artifactPath = "target/${pom.artifactId}-${pom.version}.${pom.packaging}"

                    if (fileExists(artifactPath)) {
                        echo "Uploading ${artifactPath} to Nexus"

                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [
                                    artifactId: pom.artifactId,
                                    classifier: '',
                                    file: artifactPath,
                                    type: pom.packaging
                                ],
                                [
                                    artifactId: pom.artifactId,
                                    classifier: '',
                                    file: "pom.xml",
                                    type: "pom"
                                ]
                            ]
                        )
                    } else {
                        error "Artifact not found: ${artifactPath}"
                    }
                }
            }
        }
    }
}
```
