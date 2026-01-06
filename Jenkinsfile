pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

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
                        sh 'sonar-scanner'
                    }
                }
            }
        }

        // Optional – many startups enable this later
        stage('Quality Gate') {
            when {
                expression { return env.CHANGE_ID == null } // skip for PRs
            }
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
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

