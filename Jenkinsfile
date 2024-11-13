pipeline {
    agent any
    environment {
        REPO_URL = 'https://github.com/maulin-architect/Maulin_jenkins.git'
    }
    stages {
        
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
        always {
            cleanWs() // Clean workspace after build
        }
        success {
            mail to: 'maulin.architect@gmail.com',
                 subject: "Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "The build was successful!"
        }
        failure {
            mail to: 'maulin.architect@gmail.com',
                 subject: "Build Failure: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "The build failed."
        }
    }
}
