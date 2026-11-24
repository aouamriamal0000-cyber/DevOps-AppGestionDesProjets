pipeline {
    agent any

    tools {
    jdk   'JDK17'
    maven 'Maven3'
}

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout GitHub') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/aouamriamal0000-cyber/DevOps-AppGestionDesProjets.git'
            }
        }

        stage('Tests unitaires') {
            steps {
                dir('backend') {
                    sh 'mvn clean test'
                }
            }
            post {
                always {
                    junit allowEmptyResults: true, testResults: 'backend/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package (livrable dans target)') {
            steps {
                dir('backend') {
                    sh 'mvn package -DskipTests'
                }
            }
            post {
                success {
                    archiveArtifacts artifacts: 'backend/target/*.jar', fingerprint: true
                }
            }
        }
    }

    post {
        failure {
            mail to: 'amal.aouamri@esprit.com',
                 subject: "ECHEC du build : ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """Le build a echoue.
Job : ${env.JOB_NAME}
Build : #${env.BUILD_NUMBER}
Logs : ${env.BUILD_URL}console"""
        }
    }
}
