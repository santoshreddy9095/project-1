pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven'
    }

    stages {

        stage('Build & Test') {
            steps {
                dir('project-1') {
                    sh 'mvn clean test'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                dir('project-1') {
                    withSonarQubeEnv('sonarqube') {
                        sh """
                        sonar-scanner \
                        -Dsonar.projectKey=project-1 \
                        -Dsonar.sources=src \
                        -Dsonar.java.binaries=target
                        """
                    }
                }
            }
        }

        stage('Package') {
            steps {
                dir('project-1') {
                    sh 'mvn package -DskipTests'
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'project-1/target/*.jar', fingerprint: true
            }
        }
    }

    post {
        failure {
            echo '❌ Build failed'
        }
        success {
            echo '✅ Build succeeded'
        }
    }
}

