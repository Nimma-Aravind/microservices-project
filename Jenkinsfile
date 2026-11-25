pipeline {
    agent any
    
    stages {
        stage('CQA') {
            steps {
                script {
                def scannerHome = tool 'sonar-scanner'
                withSonarQubeEnv('mysonar') {
                    sh """
                    ${scannerHome}/bin/sonar-scanner \
                    -Dsonar.projectKey=myproject \
                    -Dsonar.projectName=myproject \
                    -Dsonar.sources=.
                """
            }
        }
    }
}
        stage('Quality Gates') {
            steps {
                waitForQualityGate abortPipeline: true, credentialsId: 'sonar-cred'
            }
        }
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t nimmaaravind/frontend:latest ."
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push nimmaaravind/frontend:latest"
                    }
                }
            }
        }
    }
}
