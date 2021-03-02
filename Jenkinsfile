pipeline {
    agent {
        docker {
            image "maven:3.6.3-openjdk-17"
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
                withSonarQubeEnv('sonar') {
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