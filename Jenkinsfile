pipeline{
    agent any

    environment{
        APP_URL = 'https://gallery-practise-app-26f62825bb72.herokuapp.com/'
    }

    stages{
        stage('clone'){
            steps{
                git 'https://github.com/paulmurithi/gallery.git'
            }
        }
        stage('Build'){
            steps{
                sh 'npm install'
            }
        }
        stage('Test'){
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
            steps{
               withCredentials([usernameColonPassword(credentialsId: 'heroku-api-key', variable: 'HEROKU_CREDENTIALS')]) {
                    sh 'git push https://${HEROKU_CREDENTIALS}@git.heroku.com/gallery-practise-app.git master'
               } 
            }
        }
    }

    post{
        failure{
            slackSend color:"danger", message: "Build failed - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
        }
        success{
            slackSend color:"good", message: "Build success - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${APP_URL}|Link>)"
        }
    }
}