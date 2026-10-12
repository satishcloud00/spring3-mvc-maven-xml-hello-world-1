
pipeline {
    agent any

    tools {
        maven 'MVN_HOME'
        jdk 'Java21'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git 'https://github.com/satishcloud00/spring3-mvc-maven-xml-hello-world-1.git'
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
                withCredentials([
                    usernamePassword(
                        credentialsId: 'nexus-credentials',
                        usernameVariable: 'NEXUS_USER',
                        passwordVariable: 'NEXUS_PASS'
                    )
                ]) {
                    sh '''
                        set +x

                        SETTINGS="$WORKSPACE/nexus-settings.xml"
                        trap 'rm -f "$SETTINGS"' EXIT

                        cat > "$SETTINGS" <<EOF
<settings>
  <servers>
    <server>
      <id>nexus</id>
      <username>${NEXUS_USER}</username>
      <password>${NEXUS_PASS}</password>
    </server>
  </servers>
</settings>
EOF

                        mvn -s "$SETTINGS" deploy:deploy-file \
                          -DgroupId=com.ncodeit \
                          -DartifactId=ncodeit-hello-world \
                          -Dversion=3.0 \
                          -Dpackaging=war \
                          -Dfile=target/ncodeit-hello-world-3.0.war \
                          -DrepositoryId=nexus \
                          -Durl=http://50.17.111.39:8081/repository/maven-releases/
                    '''
                }
            }
        }
    }
}
