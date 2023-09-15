pipeline {
    agent any

    tools {
        jdk "jdk-17"
        maven "maven-3"
    }

    stages {
       stage('Clean workspace') {
            steps {
                cleanWs disableDeferredWipeout: true, deleteDirs: true
            }
        }

        stage('Pull szkolenie cicd') {
            steps {
                 git branch: 'main', url: 'https://github.com/Szwaczyn/szkolenie-cicd-jenkins-gitlab-example.git'
            }
            post {
                success {
                    slackSend message: 'Pulled szkolenie cicd', color: '#4287f5'
                }
                failure {
                    slackSend message: 'Faild'
                }
            }
        }

        stage('Build') {
            steps {
                sh "mvn clean install -DskipTests"
            }
            post {
                success {
                    slackSend message: 'Build ok'
                }
                failure {
                    slackSend message: 'Build failes'
                }
            }
        }

        stage('Tests') {
            steps {
                 sh "mvn test"
                 slackSend message: "Testy ok"
            }
        }
    }
}
