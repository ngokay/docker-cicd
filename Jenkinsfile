pipeline{
    environment{
        dockerHubCredentials = 'docker-hub'
    }
    agent any
    stages{
        stage("Test"){
            steps{
                echo "========executing A========"
            }
        }

        stage("verify tooling"){
            steps{
                bat '''
                docker version
                docker info
                docker-compose version
                curl --version
                '''
            }
        }
        stage("Prune docker data"){
            steps{
                bat 'docker system prune -a --volumes -f'
            }
        }

        stage('Docker Hub login') {
            steps {
                script {
                    // Use Jenkins credentials for Docker Hub login
                    withCredentials([usernamePassword(credentialsId: dockerHubCredentials, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        bat "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                    }
                }
            }
        }

        stage("Build container"){
            steps{
                //bat 'docker-compose up -d --no-color --wait'
                bat 'docker-compose build'
                // Push the image
                bat "docker-compose push"
                bat 'docker-compose ps'
            }
        }        
    }
    post{
        always{
            echo "========always========"
            //bat 'docker-compose down --remove-orphans -v'
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}