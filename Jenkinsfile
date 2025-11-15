pipeline {
    agent any
    
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/AymenAkhtar/task1lab10.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        
        stage('Run Tests') {
            steps {
                bat 'npm test'
            }
        }
    }
    
    post {
        success {
            echo 'Build and tests successful!'
        }
        failure {
            echo 'Build failed or tests failed!'
        }
    }
}
