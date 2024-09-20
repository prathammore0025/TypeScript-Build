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
                expression { currentBuild.currentResult == 'SUCCESS' } // Check if the current result is 'SUCCESS'
            }
            steps {
                script {
                    // Configure Git
                    sh 'git config user.email "you@example.com"'
                    sh 'git config user.name "Your Name"'
                    sh 'git add ./dist/*' // Add your build artifacts
                    sh 'git commit -m "Add new build artifacts"'
                    sh 'git push origin dev'
                }
            }
        }

        stage('Rollback') {
            when {
                expression { currentBuild.currentResult == 'FAILURE' } // Check if the current result is 'FAILURE'
            }
            steps {
                script {
                    // Perform rollback actions here
                    echo 'Rolling back to the previous build...'
                    sh 'git checkout HEAD^' // Checkout the previous commit
                }
            }
        }
    }

    triggers {
        pollSCM('H/5 * * * *')
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '2'))
    }
}
