pipeline {
    agent any

    environment {
        PHP = "/usr/local/opt/php@8.4/bin/php"
        COMPOSER = "/usr/local/bin/composer"
        NODE = "/usr/local/bin/node"
        NPM = "/usr/local/bin/npm"
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
                sh "${PHP} -v"
                sh "${PHP} ${COMPOSER} --version"
            }
        }

        stage('Install dependencies') {
            steps {
                sh "${PHP} ${COMPOSER} install --no-interaction --prefer-dist"
            }
        }

        stage('Run static analysis') {
            steps {
                sh "${PHP} vendor/bin/phpstan analyse --memory-limit=2G || true"
            }
        }

        stage('Build frontend (if exists)') {
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
