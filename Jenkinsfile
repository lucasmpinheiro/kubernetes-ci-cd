pipeline {
    agent any

    environment {
        APP_NAME = 'hello-lucasmp'
        REGISTRY_HOST = "127.0.0.1:30400"
        IMAGE_NAME = "$REGISTRY_HOST/$APP_NAME:$BUILD_NUMBER"
    }

    stages {
        stage('CI Build and push snapshot') {
            steps {
                sh "git rev-parse --short HEAD > commit-id"
                image = docker.build(imageName, "./applications/hello-kenzan")
                image.push()
            }
        }

        stage('Deploy') {
            steps {
                sh "sed 's#127.0.0.1:30400/hello-kenzan:latest#'$IMAGE_NAME'#' applications/hello-kenzan/k8s/deployment.yaml | kubectl apply -f -"
                sh "kubectl rollout status deployment/hello-kenzan"
            }
        }
    }
}
