pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
                echo 'Repository pulled successfully'
            }
        }

        stage('Prepare PHP') {
            steps {
                sh '/usr/local/opt/php@8.4/bin/php -v'
                sh '/usr/local/bin/composer --version'
            }
        }

        stage('Install dependencies') {
            steps {
                sh '/usr/local/bin/composer install --no-interaction --prefer-dist'
            }
        }

        stage('Run static analysis') {
            steps {
                sh '/usr/local/opt/php@8.4/bin/php vendor/bin/phpstan analyse --memory-limit=2G || true'
            }
        }

        stage('Build frontend (if exists)') {
            steps {
                script {
                    if (fileExists('package.json')) {
                        sh '/usr/local/bin/npm install'
                        sh '/usr/local/bin/npm run build || true'
                    } else {
                        echo 'No frontend found — skipping'
                    }
                }
            }
        }

        stage('Success') {
            steps {
                echo "FacturaCore pipeline finished successfully."
            }
        }
    }
}
