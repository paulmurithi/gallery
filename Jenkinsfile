pipeline{
    agent any

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
}