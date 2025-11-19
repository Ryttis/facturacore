pipeline {
    agent any

    environment {
        PHP_PATH = "/usr/local/opt/php@8.4/bin/php"
        COMPOSER_HOME = "${WORKSPACE}/.composer"
        COMPOSER_PHP = "/usr/local/opt/php@8.4/bin/php"
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
                sh "${PHP_PATH} -v"
                sh "COMPOSER_PHP=${PHP_PATH} /usr/local/bin/composer --version"
            }
        }

        stage('Install dependencies') {
            steps {
                sh "COMPOSER_PHP=${PHP_PATH} /usr/local/bin/composer install --no-interaction --prefer-dist"
            }
        }

        stage('Run static analysis') {
            steps {
                sh "${PHP_PATH} vendor/bin/phpstan analyse --memory-limit=2G || true"
            }
        }

        stage('Build frontend (if exists)') {
            steps {
                script {
                    if (fileExists('package.json')) {
                        sh "/usr/local/bin/npm install"
                        sh "/usr/local/bin/npm run build || true"
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
