pipeline {
    agent any
    environment {
        SONAR_TOKEN = credentials('sonarcloud-token')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('SonarCloud Analysis') {
            steps {
                withSonarQubeEnv('sonarcloud') {
                    sh '''
                    sonar-scanner \
                      -Dsonar.projectKey=vineeth394_petclinic \
                      -Dsonar.organization=vineeth394 \
                      -Dsonar.sources=src \
                      -Dsonar.host.url=https://sonarcloud.io \
                      -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 1, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }
}
