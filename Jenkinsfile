pipeline {
    agent any
    environment {
        BUILD_ID = "${env.BUILD_ID}" // Jenkins build number as the build ID
    }
    stages {
        stage('Clone Repository') {
            steps {
                // Clone the Git repository using the GitHub token
                git url: "https://github.com/amiraelkazzaz/WeatherApp.git", branch: 'main', credentialsId: '630e80e2-1717-4110-b68d-714c808ca513'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Use Docker credentials securely
                    withCredentials([
                        usernamePassword(
                            credentialsId: 'docker-hub-credentials',
                            usernameVariable: 'mirahatem',
                            passwordVariable: 'dckr_pat_3a3oXzLpxDyY9p3A0Td2oGRE9tQ'
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
                            credentialsId: 'docker-hub-credentials',
                            usernameVariable: 'mirahatem',
                            passwordVariable: 'dckr_pat_3a3oXzLpxDyY9p3A0Td2oGRE9tQ'
                        )
                    ]) {
                        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
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
    stage('Run Ansible Playbook') {
            steps {
                script {
                    echo "Running Ansible Playbook..."
                    sh "chmod 600 ./ansible/private_key_m01"
                    sh "chmod 600 ./ansible/private_key_m02"
      		    sh "export ANSIBLE_HOST_KEY_CHECKING=False" 
                    sh "ansible-playbook -i inventory playbook.yml"

                }
            }
        }
}
