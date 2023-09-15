pipeline {
    agent any

    tools {
        jdk "jdk-17"
        maven "maven-3"
    }

    triggers {
        cron('* * * * *')
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
                 sh 'echo ' + env.CHANGE_TITLE
            }
            post {
                success {
                    slackSend message: 'Pulled szkolenie cicd', color: '#4287f5'
                }
                failure {
                    slackSend message: 'Faild', color: '#ff0000'
                }
            }
        }

        stage('Build') {
            when {
                not {
                    changelog '.*^\\ci skip\\ .+$'
                }
            }
            steps {
                sh "mvn clean install -DskipTests"
            }
            post {
                success {
                    slackSend message: 'Build ok', color: '#22ff00'
                }
                failure {
                    slackSend message: 'Build failes', color: '#ff0000'
                }
            }
        }

        stage('Tests') {
            when {
                not {
                    changelog '.*^\\ci skip\\ .+$'
                }
            }
            steps {
                 sh "mvn test"
                 slackSend message: "Testy ok", color: '#22ff00'
            }
        }
    }
}
