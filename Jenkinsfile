pipeline {
    agent any
    tools {
        maven "Maven3.9"
        jdk "JDK17"
    }

    stages {
        stage('Fetch code') {
            steps {
                git branch: 'atom', url: 'https://github.com/hkhcoder/vprofile-project.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn install -DskipTests'
            }
            post {
                success {
                    echo 'Build succeeded!'
                    archiveArtifacts artifacts: '**/*.jar', fingerprint: true
                }
            }
        }

        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Checkstyle Analysis') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('Sonar code Analysis') {
            environment {
                scannerHome = tool 'Sonar6.2'
            }
            steps {
                withSonarQubeEnv('SonarServer') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
    }    
}