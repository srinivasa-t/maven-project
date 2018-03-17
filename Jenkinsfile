pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                cmd 'C:\Downloads\apache-maven-3.5.3\mvn clean package'
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