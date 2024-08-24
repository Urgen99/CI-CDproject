pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    dockerImage = docker.build("myapp:${env.BUILD_ID}")
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'echo "Running tests..."'
                        // Add test scripts here
                    }
                }
            }
        }
        
        stage('Push to Registry') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials') {
                        dockerImage.push("${env.BUILD_ID}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
        
        stage('Deploy to Staging') {
            steps {
                ansiblePlaybook credentialsId: 'ansible-credentials', playbook: 'deploy.yml'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
j
