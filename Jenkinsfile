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
                # Create build directory
                rm -rf build
                mkdir build

                # Copy all project files except the build directory itself
                rsync -av --exclude='build' . build/
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Testing PHP files for syntax errors..."
                sh '''
                find . -name "*.php" -exec php -l {} \\; | grep -v "No syntax errors"
                echo "All PHP files validated successfully!"
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying PHP website to Apache..."
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
            echo "✅ Pipeline executed successfully — Website deployed!"
        }
        failure {
            echo "❌ Pipeline failed — Check Jenkins console logs!"
        }
    }
}
