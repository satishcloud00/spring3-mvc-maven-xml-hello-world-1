pipeline {
    agent any

    tools {
        maven 'MVN_HOME'
        jdk 'Java21'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git 'https://github.com/betawins/spring3-mvc-maven-xml-hello-world-1.git'
            }
        }

        stage('Tool Install') {
            steps {
                sh 'mvn -version'
                sh 'java -version'
            }
        }

        stage('Clone Code') {
            steps {
                echo 'Source code checked out successfully'
                sh 'ls -la'
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Publish to Nexus') {
            steps {
                script {
                    def groupId = 'com.ncodeit'
                    def artifactId = 'ncodeit-hello-world'
                    def version = '3.0'
                    def packaging = 'war'

                    echo "Group ID: ${groupId}"
                    echo "Artifact ID: ${artifactId}"
                    echo "Version: ${version}"

                    sh """
                        mvn deploy:deploy-file \
                        -DgroupId=${groupId} \
                        -DartifactId=${artifactId} \
                        -Dversion=${version} \
                        -Dpackaging=${packaging} \
                        -Dfile=target/${artifactId}-${version}.war \
                        -DrepositoryId=nexus \
                        -Durl=http://YOUR_NEXUS_HOST:8081/repository/YOUR_MAVEN_REPOSITORY/
                    """
                }
            }
        }
    }
}
