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
                withSonarQubeEnv('b5251c4a882acf3c22792f451630e67eec0fcb12') {
                    sh '''
                    sonar-scanner \
                      -Dsonar.projectKey=vineeth394_petclinic \
                      -Dsonar.organization=vineeth394 \
                      -Dsonar.sources=src \
                      -Dsonar.host.url=https://sonarcloud.io \
                      -Dsonar.login=d9da154c8af16edfceb4fd3247cb613ea27c14c0
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
