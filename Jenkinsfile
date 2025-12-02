pipeline {
    agent any

    environment {
        APP_DIR = "/opt/app"   // Deployment directory outside Jenkins
    }

    stages {

        stage('Clean Workspace') {
            steps {
                deleteDir()   // Safe Jenkins cleanup
            }
        }

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/mannankazi/Practice-flask-app.git', branch: 'master'
            }
        }

        stage('Build Docker Image (no cache)') {
            steps {
                sh 'docker build --no-cache -t my-app:latest .'
            }
        }

        stage('Run Tests') {
            steps {
                echo "Running tests..."
                // insert test steps when ready
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'DockerHubCredentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker tag my-app:latest $DOCKER_USER/my-app:latest
                    docker push $DOCKER_USER/my-app:latest
                    '''
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                sh '''
                docker compose up -d --build --force-recreate --remove-orphans
                '''
            }
        }
    }
}
