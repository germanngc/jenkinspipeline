pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'source /etc/profile.d/maven.sh; mvn clean package'
            }

            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}

