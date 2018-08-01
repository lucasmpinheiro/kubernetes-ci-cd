node {
    checkout scm

    sh "git rev-parse --short HEAD > commit-id"

    appName = "hello-kenzan"
    registryHost = "127.0.0.1:30400/"
    imageName = "${registryHost}${appName}:${env.BUILD_ID}"
    env.BUILDIMG=imageName

    stage "Build"
        def image = docker.build(imageName, "./applications/hello-kenzan")

    stage "Push"
        image.push()

    stage "Deploy"
        sh "sed 's#127.0.0.1:30400/hello-kenzan:latest#'$BUILDIMG'#' applications/hello-kenzan/k8s/deployment.yaml | kubectl apply -f -"
        sh "kubectl rollout status deployment/hello-kenzan"
}
