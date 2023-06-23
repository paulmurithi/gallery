pipeline{
    agent any

    tools {
        nodejs '18.16.0'
    }
    
    stages{
        stage('Build'){
            steps{
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Test'){
            steps{
                sh 'npm run test'
            }
        }
        stage('Deploy'){
            steps{
                sh 'rander deploy --app-name your-app-name --path /path/to/your/app'
            }
        }
    }
}