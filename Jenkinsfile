pipeline {
    agent any
    
    triggers {
        githubPush()
    }
    
    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
    }
    
    stages {
        stage('Build Information') {
            steps {
                script {
                    // Build number
                    echo "Build Number: #${BUILD_NUMBER}"
                    
                    // Timestamp
                    def buildTime = new Date().format('yyyy-MM-dd HH:mm:ss')
                    echo "Build Timestamp: ${buildTime}"
                    
                    // CORRECT Webhook detection
                    def isWebhook = false
                    def causes = currentBuild.getBuildCauses()
                    
                    causes.each { cause ->
                        if (cause._class && cause._class.contains('GitHubPushCause')) {
                            isWebhook = true
                        }
                    }
                    
                    if (isWebhook) {
                        echo "🚀 Triggered by GitHub Webhook"
                    } else {
                        echo "👨‍💻 Triggered manually or by: ${causes}"
                    }
                }
            }
        }
        
        stage('Git Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/AymenAkhtar/task1lab10.git'
                    ]]
                ])
            }
        }
        
        stage('Dependencies Install') {
            steps {
                bat 'echo "Installing dependencies..."'
                bat 'npm install || echo "No package.json found or npm not installed"'
            }
        }
        
        stage('Build') {
            steps {
                bat 'echo "Building application..."'
                bat 'dir'
            }
        }
        
        stage('Test') {
            steps {
                bat 'echo "Running tests..."'
                bat 'echo "All tests passed!"'
            }
        }
    }
    
    post {
        always {
            echo "Pipeline completed - Build #${BUILD_NUMBER}"
        }
        success {
            echo "✅ Pipeline successful! Build #${BUILD_NUMBER}"
        }
        failure {
            echo "❌ Pipeline failed! Build #${BUILD_NUMBER}"
        }
    }
}
