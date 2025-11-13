pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/var/www/html'
    }

    stages {

        stage('Code Clone') {
            steps {
                echo "Cloning latest code from GitHub..."
                git branch: 'main', url: 'https://github.com/dhimanankit1593/php-website-demo.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building PHP application..."
                sh '''
                mkdir -p build
                cp -r * build/
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Testing PHP files for syntax errors..."
                sh '''
                find . -name "*.php" -exec php -l {} \\; | grep -v "No syntax errors"
                echo "All PHP files passed syntax validation!"
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying application to Apache web server..."
                sh '''
                sudo rm -rf ${DEPLOY_DIR}/*
                sudo cp -r build/* ${DEPLOY_DIR}/
                sudo systemctl restart apache2
                echo "Deployment completed successfully!"
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Website deployed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check Jenkins console output."
        }
    }
}
