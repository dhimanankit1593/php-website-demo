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
                rm -rf build
                mkdir build
                rsync -av --exclude='build' --exclude='.git' . build/
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Testing PHP files for syntax errors..."
                sh '''
                # Run syntax check for all PHP files
                ERRORS=0
                for file in $(find . -name "*.php"); do
                    php -l $file || ERRORS=1
                done

                if [ $ERRORS -ne 0 ]; then
                    echo "PHP syntax errors detected!"
                    exit 1
                else
                    echo "All PHP syntax tests passed successfully ✅"
                fi
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
