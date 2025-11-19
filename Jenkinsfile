pipeline {
    agent any

    tools {
        // Use Git installed on macOS
        git 'Default'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
                echo 'Repository pulled successfully'
            }
        }

        stage('Prepare PHP') {
            steps {
                sh 'php -v'
                sh 'composer --version'
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'composer install --no-interaction --prefer-dist'
            }
        }

        stage('Run static analysis') {
            steps {
                sh 'vendor/bin/phpstan analyse --memory-limit=2G || true'
            }
        }

        stage('Build frontend (if exists)') {
            steps {
                script {
                    if (fileExists('package.json')) {
                        sh 'npm install'
                        sh 'npm run build || true'
                    } else {
                        echo 'No frontend found — skipping'
                    }
                }
            }
        }

        stage('Success') {
            steps {
                echo "FacturaCore pipeline finished."
            }
        }
    }
}
