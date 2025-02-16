pipeline {
    agent any
    environment {
        BUILD_ID = "${env.BUILD_ID}" // Jenkins build number as the build ID
    }
    stages {
        stage('Clone Repository') {
            steps {
                // Clone the Git repository using the GitHub token
                git url: "https://github.com/amiraelkazzaz/WeatherApp.git", branch: 'main', credentialsId: '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'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Use Docker credentials securely
                    withCredentials([
                        usernamePassword(
                            credentialsId: 'dckr_pat_d8L3lQQgyVCMNRPlfZhB7jifA-8',
                            usernameVariable: 'DOCKER_USERNAME',
                            passwordVariable: 'DOCKER_PASSWORD'
                        )
                    ]) {
                        // Build the Docker image with the build ID as the tag
                        docker.build("mirahatem/simple-web-app:${BUILD_ID}")
                    }
                }
            }
        }
        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub and push the image
                    withCredentials([
                        usernamePassword(
                            credentialsId: 'dckr_pat_d8L3lQQgyVCMNRPlfZhB7jifA-8',
                            usernameVariable: 'DOCKER_USERNAME',
                            passwordVariable: 'DOCKER_PASSWORD'
                        )
                    ]) {
                        docker.withRegistry('https://registry.hub.docker.com', 'dckr_pat_d8L3lQQgyVCMNRPlfZhB7jifA-8') {
                            docker.image("mirahatem/simple-web-app:${BUILD_ID}").push()
                        }
                    }
                }
            }
        }
        stage('Deploy with Ansible') {
            steps {
                // Run the Ansible playbook to deploy the application
                sh """
                # Ensure the inventory file and playbook are present
                ansible-playbook -i inventory.ini playbook.yml --extra-vars "build_id=${BUILD_ID}"
                """
            }
        }
    }
    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs for details."
        }
    }
}
