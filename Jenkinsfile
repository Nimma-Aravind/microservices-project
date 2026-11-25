pipeline {
    agent any
    tools {
        maven 'mymaven'
    }

    stages {
        stage('CQA') {
            steps {
                withSonarQubeEnv('mysonar') {
                sh "mvn clean verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=myproject -Dsonar.projectName=myproject"
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
