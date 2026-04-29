pipeline {
    agent any

    tools {
        maven 'maven3'
        jdk 'jdk17'  
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/chimdi247/Boardgame.git'
            }
        }

        stage('Compilation') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Testing') {
            steps {
                sh 'mvn test'
            }
        }
         stage('Build') {
            steps {
                sh 'mvn package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''
                        $SCANNER_HOME/bin/sonar-scanner \
                            -Dsonar.projectName=Boardgame \
                            -Dsonar.projectKey=Boardgame \
                            -Dsonar.java.binaries=target
                    '''
                }
            }
        }
          stage('Quality Gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
    }
}
