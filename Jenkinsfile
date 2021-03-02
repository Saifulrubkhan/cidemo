pipeline {
    agent {
        docker {
            image "maven:3.6.3-jdk-8"
        }
    }
    triggers {
        pollSCM("* * * * *")
    }
    stages {
        stage('Prepration') {
            steps {
                cleanWs()
            }
        }
        stage('Declrative checkout') {
            steps {
                sh "echo checking out code"
                checkout scm
            }
        }
        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }
        stage('Testing') {
            steps {
                sh "mvn test"
            }
        }
        stage('Publish test result') {
            steps {
                junit '**/surefire-reports/*.xml'
            }
        }
        stage('Code quality check') {
            steps {
                withSonarQubeEnv(credentialsId: 'sonar') {
                    sh "mvn sonar:sonar"
                    // some block
                }
            }
        }

    }
    post {
        always {
            echo " Looks Good"
        }
    }

}