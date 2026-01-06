pipeline {
    agent any

    tools {
        jdk 'jdk17'
	maven 'maven'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/santoshreddy9095/project-1.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                dir('project-1') {
                    withSonarQubeEnv('sonarqube') {
                	sh '''
                  	  mvn clean verify \
                  	  org.sonarsource.scanner.maven:sonar-maven-plugin:sonar
                	'''
            	    }
        	}
    	    }
	}

        stage('Build with Maven') {
            steps {
                dir('project-1') {
                    sh 'mvn clean package'
                }
            }
         }

        stage('Archive Artifact') {
    	    steps {
                dir('project-1') {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        	}
    	    }
	}

    }
}



