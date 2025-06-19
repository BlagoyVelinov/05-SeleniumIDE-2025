pipeline {
    agent any

    stages {
        stage('Checkout the repository') {
            steps {
                checkout scm
            }
        }

        stage('Restore the project') {
            steps {
                bat 'dotnet restore'
            }
        }

        stage('Build the project') {
            steps {
                bat 'dotnet build --configuration Release'
            }
        }

        stage('Test') {
            steps {
                bat 'dotnet test --no-build --configuration Release --logger "trx;LogFileName=test_results.trx" --results-directory TestResults'
            }
        }

        stage('Publish Artifacts') {
            steps {
                bat 'dotnet publish SeleniumIDE/SeleniumIde.csproj -c Release -o publish'
                archiveArtifacts artifacts: 'publish/**', fingerprint: true
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'TestResults/*.trx', fingerprint: true

            cleanWs()
        }
    }
}
