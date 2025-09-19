pipeline {
    agent any
    environment {
        DOCKER_PWD = credentials('docker-login-pwd')  // Docker Hub password
        GITHUB_TOKEN = credentials('git-token')      // GitHub PAT
        DOCKER_USER = 'mkrai'                         // Your Docker Hub username
        REPO_URL = 'https://github.com/Manish-Kumar-Rai/jenkins-pipeline.git'
    }
    stages {
        stage("Checkout GitHub") {
            steps {
                echo "Checking out GitHub repository..."
                sh """
                    git clone https://\$GITHUB_TOKEN@github.com/Manish-Kumar-Rai/jenkins-pipeline.git repo
                    cd repo
                    git status
                """
            }
        }
        stage("Docker Login") {
            steps {
                echo "Logging into Docker Hub..."
                sh "echo \$DOCKER_PWD | docker login -u \$DOCKER_USER --password-stdin"
                sh "docker info"
            }
        }
    }
}
