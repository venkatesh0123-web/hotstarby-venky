pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from main branch
                git branch: 'main', url: 'https://github.com/Harsha6404/hotstarby.git'

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

        stage('artifact') {
            steps {
               nexusArtifactUploader artifacts: [[artifactId: 'myapp', classifier: '', file: '**/*.war', type: 'war']], credentialsId: 'nexuscreds', groupId: 'in.reyaz', nexusUrl: '13.51.197.175:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'hotspot', version: '8.3.3-SNAPSHOT'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker rmi -f hotstar:v1 || true
                    docker build -t hotstar:v1 -f /var/lib/jenkins/workspace/hotspot/Dockerfile /var/lib/jenkins/workspace/hotspot
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f con8 || true
                    docker run -d --name con8 -p 8008:8080 hotstar:v1
                '''
            }
        }

        stage('Docker Swarm Deploy') {
            steps {
                sh '''
                    docker service update --image hotstar:v1 hotstarserv || \
                    docker service create --name hotstarserv -p 8009:8080 --replicas=10 hotstar:v1
                '''
            }
        }
    }
}
