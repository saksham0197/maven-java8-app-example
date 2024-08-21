pipeline {
    agent any

    tools {
        maven 'Maven 3.x'
    }

    environment {
        SONARQUBE_SCANNER_HOME = tool name: 'SonarScanner'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/saksham0197/maven-java8-app-example.git', branch: 'master'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    withCredentials([string(credentialsId: 'sonar_secret', variable: 'sonar_secret')]) {
                        sh "mvn sonar:sonar \
                            -Dsonar.projectKey=saksham0197_Rest_Api_projects_SpringBoot \
                            -Dsonar.organization=saksham0197 \
                            -Dsonar.host.url=https://sonarcloud.io \
                            -Dsonar.login=${SONAR_TOKEN}"
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }
}
