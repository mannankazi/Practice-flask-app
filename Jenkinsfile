pipeline {
    agent any

    stages {
        stage('Code Clone') {
            steps {
                git url: 'https://github.com/mannankazi/Practice-flask-app.git', branch: 'master'
            }
        }

        stage('Build Image (no cache)') {
            steps {
                // Force fresh build every time – no Docker layer cache at all
                sh 'docker build --no-cache --pull -t my-app .'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing phase – add real tests later'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'DockerHubCredentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    sh 'docker tag my-app $DOCKER_USER/my-app:V1'
                    sh 'docker push $DOCKER_USER/my-app:V1'
                }
            }
        }

        stage('Deploy – Fresh Everything') {
            steps {
                // Stop and remove everything (containers, networks, volumes)
                sh 'docker compose down -v --remove-orphans || true'

                // Rebuild image from scratch + start fresh containers
                sh 'docker compose up -d --build --force-recreate  --renew-anon-volumes'
            }
        }
    }
}
