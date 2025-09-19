pipeline {
    environment {
        registry = "mkrai/jenkins-docker-test"
        DOCKER_PWD = credentials('docker-login-pwd')   // Docker Hub password
        GITHUB_TOKEN = credentials('git-token')       // GitHub PAT
    }
    agent {
        docker {
            image 'mkrai/node-docker'
            args '-p 3000:3000 -w /app -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage("Checkout") {
            steps {
                // Clone your GitHub repo using PAT
                sh '''
                    rm -rf app
                    git clone https://github.com/Manish-Kumar-Rai/jenkins-pipeline.git app
                    cd app
                '''
            }
        }
        stage("Build") {
            steps {
                sh 'cd app && npm install'
            }
        }
        stage("Test") {
            steps {
                sh 'cd app && npm test'
            }
        }
        stage("Build & Push Docker image") {
            steps {
                sh 'docker image build -t $registry:$BUILD_NUMBER app'
                sh 'docker login -u mkrai -p $DOCKER_PWD'
                sh 'docker image push $registry:$BUILD_NUMBER'
                sh "docker image rm $registry:$BUILD_NUMBER"
            }
        }
        stage("Deploy and smoke test") {
            steps {
                sh 'sh ./jenkins/scripts/deploy.sh'
            }
        }
        stage("Cleanup") {
            steps {
                sh 'sh ./jenkins/scripts/cleanup.sh'
            }
        }
    }
}
