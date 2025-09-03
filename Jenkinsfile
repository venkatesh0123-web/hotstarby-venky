pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('venky')  
        // Jenkins credentials ID for Docker Hub (username+password or token)
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/venkatesh0123-web/hotstarby-venky.git'

                sh 'pwd'
                sh 'ls -l'
                sh 'ls -R'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker rmi -f hotstar:v1 || true
                    docker build -t hotstar:v1 -f Dockerfile .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'venky', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker tag hotstar:v1 $DOCKER_USER/hotstar:v1
                        docker push $DOCKER_USER/hotstar:v1
                        docker logout
                    '''
                }
            }
        }

        stage('Docker Swarm Deploy') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'venky', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        docker service update --image $DOCKER_USER/hotstar:v1 hotstarserv || \
                        docker service create --name hotstarserv -p 8009:8080 --replicas=10 $DOCKER_USER/hotstar:v1
                    '''
                }
            }
        }
    }
}
