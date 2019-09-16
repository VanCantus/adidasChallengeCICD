pipeline {
    agent any

    triggers {
    //    cron('H */12 * * *') // build every half a day
        pollSCM('*/5 * * * *') // polling for changes every 5 minutes
    }

    stages {
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/paulvassu/adidasChallenge.git"
            }
        }

        stage ('Build') {
            steps {
                script {
                    sh './gradlew build'
                }
            }
        }

        stage ('Test') {
            steps {
                script {
                    sh './gradlew test'
                }
            }
        }
    }
}
