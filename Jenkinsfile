pipeline {
    agent any

    environment {
        GITHUB_TOKEN = credentials('GITHUB_TOKEN') // Use the credentials ID created in Jenkins
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
                    // Build the project and handle errors
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        sh 'npm run build'
                    }
                }
            }
        }

        stage('Run Compiled Output') {
            when {
                expression { currentBuild.currentResult == 'SUCCESS' } // Check if the current result is 'SUCCESS'
            }
            steps {
                script {
                    // Run the compiled project
                    sh 'node dist/index.js' // Update if necessary, based on your output directory
                }
            }
        }

        stage('Push Artifact') {
            when {
                expression { currentBuild.result == 'SUCCESS' } // Check if the build succeeded
            }
            steps {
                script {
                    // Configure Git with user details
                    sh 'git config user.email "you@example.com"'
                    sh 'git config user.name "Your Name"'
                    sh 'git remote set-url origin https://$GITHUB_TOKEN@github.com/prathammore0025/TypeScript-Build.git'
                    sh 'git add dist/*' // Add your build artifacts from the correct folder
                    sh 'git commit -m "Add new build artifacts"'
                    sh 'git push origin dev'
                }
            }
        }

        stage('Rollback') {
            when {
                expression { curr
