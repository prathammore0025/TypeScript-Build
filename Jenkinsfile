pipeline {
    agent {
        docker {
            image 'node:16'  // Use Node.js 16.x Docker image
            args '-u root'   // Run as root user to avoid permission issues
        }
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/prathammore0025/TypeScript-Build.git', branch: 'dev'
            }
        }

        stage('Setup Environment') {
            steps {
                script {
                    // Install dependencies
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build the project
                    sh 'npm run build'
                }
            }
        }
    }

    triggers {
        pollSCM('H/5 * * * *')
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
}
