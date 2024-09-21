pipeline{
    environment{
        DOCKER_IMAGE = "anhnt/testcicd-docker"
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
                bat 'docker-compose down'
                bat 'docker rm -f $(docker ps -a -q)'
                bat 'docker volume rm $(docker volume ls -q)'
            }
        }

        stage("Start container"){
            steps{
                bat 'docker-compose up -d --no-color --wait'
                bat 'docker-compose ps'
            }
        }
        stage("Run test"){
            steps{
                bat 'curl http://localhost:5000'
            }
        }

    }
    post{
        always{
            echo "========always========"
            //bat 'docker-compose down --remove-orphans -v'
            bat 'docker-compose ps'
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}