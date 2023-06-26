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
        }
        stage('Deploy'){
            steps{
               withCredentials([usernameColonPassword(credentialsId: 'heroku-api-key', variable: 'HEROKU_CREDENTIALS')]) {
                    sh 'git push https://${HEROKU_CREDENTIALS}@git.heroku.com/gallery-practise-app.git master'
               } 
            }
        }
    }
    post {
    always {
      emailext (
        to: 'murithi.paul@student.moringaschool.com',
        subject: 'Build Notification - ${currentBuild.fullDisplayName}',
        body: '''<p>Build Status: ${currentBuild.result}</p>
                 <p>Build URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>''',
        attachLog: true
      )
    }
  }
}