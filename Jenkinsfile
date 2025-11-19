pipeline {
    agent any

    environment {
        PHP = "/usr/local/opt/php@8.4/bin/php"
        NODE = "/usr/local/bin/node"
        NPM = "/usr/local/bin/npm"
        PATH = "/usr/local/bin:/opt/homebrew/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Download Composer') {
            steps {
                sh """
                    rm -f composer.phar
                    curl -sS https://getcomposer.org/download/2.8.12/composer.phar -o composer.phar
                    ${PHP} composer.phar --version
                """
            }
        }

        stage('Install dependencies') {
            steps {
                sh "${PHP} composer.phar install --no-interaction --prefer-dist"
            }
        }

        stage('Run static analysis') {
            steps {
                sh "${PHP} vendor/bin/phpstan analyse --memory-limit=2G"
            }
        }

        stage('Build frontend') {
            steps {
                script {
                    if (fileExists('package.json')) {
                        sh "${NPM} install"
                        sh "${NPM} run build || true"
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

