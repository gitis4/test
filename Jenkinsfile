pipeline {
    agent any

    options {
        skipStagesAfterUnstable()
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Vérification du code source...'
                checkout scm 
            }
        }

        stage('Fix Windows EBUSY)') {
            steps {
                echo 'Nettoyage du fichier de base de données'
                bat 'IF EXIST C:\\etc\\todos\\todo.db (del /F /Q C:\\etc\\todos\\todo.db) ELSE (echo DB file not found, continuing...)'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Tests') {
            steps {
                bat 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t todo-app .'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline réussi !'
        }
        failure {
            echo 'Pipeline échoué !'
        }
    }
}