pipeline {

    agent { label 'knox' }

    stages {

        stage('Code') {
            steps {
                echo "Cloning the repository"
                git branch: 'main',
                    url: 'https://github.com/Rupesh-knox/django-notes-app.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building Docker image"
                sh "docker build -t notes-app:latest ."
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo "Pushing image to DockerHub"

                withCredentials([usernamePassword(
                    credentialsId: 'dockerHubCredentials',
                    usernameVariable: 'dockerHubUser',
                    passwordVariable: 'dockerHubPass'
                )]) {

                    sh """
                    docker login -u rupeshs11 -p ${dockerHubPass}
                    docker tag notes-app:latest rupeshs11/notes-app:latest
                    docker push rupeshs11/notes-app:latest
                    """
                }
            }
        }

        stage('Test') {
            steps {
                echo "Running test container"
                sh """
                docker compose up -d
                """
            }
        }

        stage('Deploy') {
            steps {
                echo "Deployment Successful"
            }
        }
    }
}
