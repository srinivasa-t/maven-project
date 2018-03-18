pipeline {
    agent any

    parameters {
        string(name: 'tomcat_staging', defaultValue: '35.231.13.157', description:'Tomcat Staging IP')
        string(name: 'tomcat_prod', defaultValue: '35.196.174.31', description:'Tomcat Production IP')
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages{
        stage('Build'){
            steps {

                bat 'mvn clean package'
          }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deployments') {
            parallel {
                stage('Deploy to Staging') {
                    steps {
                        echo ${params.tomcat_staging}
                        sh "cp **/target/*.war C:\\Downloads\\apache-tomcat-9.0.6\\webapps\\"
                    }
                }
                stage('Deploy to Production') {
                    steps {
                        echo ${params.tomcat_prod}
                        timeout(time: 5, unit: 'DAYS') {
                            input message: 'Approve Production Deployment?'
                        }
                        sh "cp **/target/*.war C:\\Downloads\\Production-Tomcat\\webapps\\"
                    }
                    post {
                            success {
                                echo 'Code deployed to production.'
                            }
                            failure {
                                echo 'Deployment to Production failed.'    
                            }
                    }
                }
            }
        }
    }
}
