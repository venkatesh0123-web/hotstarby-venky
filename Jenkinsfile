pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from main branch
                git branch: 'main', url: 'https://github.com/prasanth-1243/hotstarby.git'

                // Verify files
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
 feature-hotstar-webpage
     

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker rmi -f hotstar:v1 || true
                    docker build -t hotstar:v1 -f /var/lib/jenkins/workspace/hoststar_by_venky/Dockerfile /var/lib/jenkins/workspace/hoststar_by_venky
                '''
            }
        }


        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f con8 || true
 feature-hotstar-webpage
                    docker run -d --name con8 -p 9943:8080 hotstar:v1
                '''
            }
        }

                    docker run -d --name con8 -p 8008:8080 hotstar:v1
                '''
            }
        }
    }
}
