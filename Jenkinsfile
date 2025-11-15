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
                    
                    // Webhook trigger check
                    if (env.GIT_URL || env.BUILD_CAUSE == "githubhook") {
                        echo "🚀 Triggered by GitHub Webhook"
                    } else {
                        echo "👨‍💻 Triggered manually"
                    }
                    
                    // Build cause bhi print karein
                    echo "Build Cause: ${currentBuild.getBuildCauses()}"
                }
            }
        }
        
        stage('Git Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/AymenAkhtar/task1lab10.git',
                        credentialsId: ''
                    ]]
                ])
            }
        }
        
        stage('Dependencies Install') {
            steps {
                sh 'echo "Installing dependencies..."'
                // Agar package.json hai toh
                sh 'npm install || echo "No package.json found"'
            }
        }
        
        stage('Build') {
            steps {
                sh 'echo "Building application..."'
                sh 'ls -la'
            }
        }
        
        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
            }
        }
    }
    
    post {
        success {
            echo "✅ Pipeline successful! Build #${BUILD_NUMBER}"
            echo "📅 Completed at: ${new Date().format('yyyy-MM-dd HH:mm:ss')}"
        }
        failure {
            echo "❌ Pipeline failed! Build #${BUILD_NUMBER}"
        }
    }
}