pipeline{
    agent any

    environment{
        APP_URL = 'https://gallery-practise-app-26f62825bb72.herokuapp.com/'
    }

    stages{

        slackSend (color: '#FFFF00', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

        stage('clone'){
            steps{
                git 'https://github.com/paulmurithi/gallery.git'
            }
        }
        stage('Build'){

            slackSend (color: '#FFFF00', message: "Building: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

            steps{
                sh 'npm install'
            }
        }
        stage('Test'){

            slackSend (color: '#FFFF00', message: "TESTING: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

            steps{
                sh 'npm test'
            }
            post {
                failure {
                    emailext (
                        subject: "Test stage failed: ${currentBuild.fullDisplayName}",
                        body: """<p>The test stage has failed in the build:</p>
                                <p>Build URL: <a href='${env.BUILD_URL}'>${env.BUILD_URL}</a></p>""",
                        to: 'murithi.paul@student.moringaschool.com'
                    )
                }
            }
        }
        stage('Deploy'){

            slackSend (color: '#FFFF00', message: "DEPLOYING: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
            
            steps{
               withCredentials([usernameColonPassword(credentialsId: 'heroku-api-key', variable: 'HEROKU_CREDENTIALS')]) {
                    sh 'git push https://${HEROKU_CREDENTIALS}@git.heroku.com/gallery-practise-app.git master'
               } 
            }
        }
    }

    post{
        failure{
            slackSend color:"danger", message: "Build failed. Job name: ${env.JOB_NAME} build id: ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
        }
        success{
            slackSend color:"good", message: "Build success. Job name: ${env.JOB_NAME} build id: ${env.BUILD_NUMBER} (<${APP_URL}|App link>)"
        }
    }
}