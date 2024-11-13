pipeline {
    agent any
    environment {
        REPO_URL = 'https://github.com/maulin-architect/Maulin_jenkins.git'
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the correct branch based on trigger
                    checkout scm
                }
            }
        }

        
        stage('Build') {
            steps {
                echo ' Building ...'
               // sh 'make build' // Or another build command
            }
        }

        stage('Unit Tests') {
            steps {
                 echo ' Testing ...'
               // sh 'make test-unit' // Run unit tests
            }
        }

        stage('Static Code Analysis') {
            steps {
                echo 'static code analysis'
                // sh 'make lint' // Or another static analysis tool
            }
        }

        stage('Deploy to Staging') {
            when {
                branch 'develop'
            }
            steps {
                sh 'make deploy-staging' // Deploys develop branch to staging
            }
        }

        stage('Integration Tests') {
            when {
                branch 'develop'
            }
            steps {
                sh 'make test-integration' // Run integration tests on staging
            }
        }

        stage('Create Release Artifact') {
            when {
                branch pattern: "release/.*", comparator: "REGEXP"
            }
            steps {
                sh 'make build-release' // Build the release artifact
                archiveArtifacts artifacts: 'build/*.jar', fingerprint: true
            }
        }

        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {

                echo 'Deploying'
               // sh 'make deploy-prod' // Deploy main branch to production
            }
        }
    }

   post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
