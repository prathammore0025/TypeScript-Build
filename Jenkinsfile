pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/prathammore0025/TypeScript-Build.git', branch: 'dev'
            }
        }

        stage('Setup Environment') {
            steps {
                script {
                    // Clean the build directory if it exists
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Clean previous builds and build the project
                    sh 'npm run build'
                }
            }
            post {
                success {
                    echo 'Build succeeded!'
                }
                failure {
                    echo 'Build failed. Rolling back...'
                    // Optionally, you can implement a rollback strategy here
                }
            }
        }
    }

    options {
        // Keep the last 5 builds
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
}
