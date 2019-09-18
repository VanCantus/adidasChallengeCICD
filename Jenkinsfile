pipeline {
    agent any

    // Define variables for use in the different stages of the Jenkins pipeline
    environment {
        MS_NAME = 'adidas-challenge'
        REPO = 'vancantus'
        MS_IMAGE_TAG = "${REPO}/${MS_NAME}:v${env.BUILD_NUMBER}"
        MS_DOCKERFILE_NAME = 'Dockerfile-ms'
        MS_CONTAINER_NAME = 'ms-adidas-challenge'
    }

    triggers {
    //    cron('H */12 * * *') // build every half a day
        pollSCM('*/5 * * * *') // polling for changes every 5 minutes
    }

    stages {
        stage ('Checkout from GitHub') {
            steps {
                git branch: 'master', url: "https://github.com/paulvassu/adidasChallenge.git"
            }
        }

        stage ('Build Microservice') {
            steps {
                script {
                    sh './gradlew build'
                }
            }
        }

        stage ('Unit Tests') {
            steps {
                script {
                    sh './gradlew test'
                }
            }
        }

        // Add stage for further tests if needed (e.g. system integration or e2e test)

        stage ('Build Docker Image') {
            container ('docker') {
                sh "docker build -f ${MS_DOCKERFILE_NAME} -t ${MS_IMAGE_TAG} ."
            }

            /*container ('helm') {
                sh 'helm upgrade --install --force '
            }*/
        }
    }
}
