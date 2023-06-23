pipeline{
    agent any

    tools {
        nodejs '18.16.1'
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
                
            }
        }
    }
}