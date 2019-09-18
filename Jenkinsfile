podTemplate(label: 'k8s', containers: [
  containerTemplate(name: 'gradle', image: 'gradle:jdk12', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.8.8', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:latest', command: 'cat', ttyEnabled: true)
],
volumes: [
  hostPathVolume(mountPath: '/home/gradle/.gradle', hostPath: '/tmp/jenkins/.gradle'),
  hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
]) {
    node ('k8s') {
        // Define variables for use in the different stages of the Jenkins pipeline
        def ms_name = 'adidas-challenge'
        def repo = 'vancantus'
        def ms_image_tag = "${repo}/${ms_name}:v${env.BUILD_NUMBER}"
        def ms_dockerfile_name = 'Dockerfile-ms'
        def ms_container_name = 'ms-adidas-challenge'

        echo 'test'
        stage ('Checkout from GitHub') {
            echo 'Checkout from GitHub'
            git(
                url: 'https://github.com/paulvassu/adidasChallenge',
                branch: 'master'
            )
        }

        stage ('Build Microservice') {
            echo 'Build Microservice'
            script {
                sh './gradlew build'
            }
        }

        stage ('Unit Tests') {
            container ('gradle') {
                echo 'Unit Tests'
                script {
                    sh 'gradle test'
                }
            }
        }

        // Add stage for further tests if needed (e.g. system integration or e2e test)

        stage ('Build Docker Image') {
            container ('docker') {
                echo 'Build Docker Image'
                dockerImage = docker.build("${ms_image_tag}", "${ms_dockerfile_name}")
            }
        }

        stage ('Deploy to Kubernetes') {
            container ('kubectl') {
                echo 'Deploy to Kubernetes'
                sh ("kubectl set image deployment/${K8S_DEPLOYMENT_NAME} ${K8S_DEPLOYMENT_NAME}=${DOCKER_HUB_ACCOUNT}/${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}")
            }
        }
    }
}
