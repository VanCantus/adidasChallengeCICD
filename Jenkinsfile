pipeline {
    // Define variables for use in the different stages of the Jenkins pipeline
    def ms_name = 'adidas-challenge'
    def repo = 'vancantus'
    def ms_image_tag = "${repo}/${ms_name}:v${env.BUILD_NUMBER}"
    def ms_dockerfile_name = 'Dockerfile-ms'
    def ms_container_name = 'ms-adidas-challenge'

    agent any

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
                sh "docker build -f ${ms_dockerfile_name} -t ${ms_image_tag} ."
            }

            /*container ('helm') {
                sh 'helm upgrade --install --force '
            }*/
        }
    }
}
