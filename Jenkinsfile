pipeline {
    agent any

    environment {
        // Define environment variables
        DOCKER_HUB_CREDENTIALS = credentials('dckr_pat_d8L3lQQgyVCMNRPlfZhB7jifA-8') // Docker Hub credentials ID
        GIT_TOKEN = credentials('SHA256:20V/zQoeh0ABeUme60eMbuwLICHlkiFkE3HY9wB6wJA') // GitHub token credentials ID
        BUILD_ID = "${env.BUILD_ID}" // Jenkins build number as the build ID
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the Git repository using the GitHub token
                git url: "https://github.com/amiraelkazzaz/WeatherApp.git", branch: 'main', credentialsId: 'github-token'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image with the build ID as the tag
                    docker.build("mirahatem/simple-web-app:${BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub and push the image
                    docker.withRegistry('https://registry.hub.docker.com', 'dckr_pat_d8L3lQQgyVCMNRPlfZhB7jifA-8') {
                        docker.image("mirahatem/simple-web-app:${BUILD_ID}").push()
                    }
                }
            }
        }

        stage('Deploy with Ansible') {
            steps {
                // Run the Ansible playbook to deploy the application
                sh '''
                # Ensure the inventory file and playbook are present
                ansible-playbook -i inventory.ini playbook.yml --extra-vars "build_id=${BUILD_ID}"
                '''
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
