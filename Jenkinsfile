pipeline {
    agent any

    environment {
        DEPLOY_PATH = "/var/www/html"
     
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
                    rsync -av --exclude=build --exclude=.git . build/
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Testing PHP files for syntax errors..."
                sh '''
                    ERRORS=0
                    for file in $(find . -name "*.php"); do
                        php -l "$file" || ERRORS=$((ERRORS+1))
                    done
                    if [ $ERRORS -ne 0 ]; then
                        echo "❌ PHP syntax errors found!"
                        exit 1
                    fi
                    echo "✅ All PHP syntax tests passed successfully"
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying PHP website to Apache..."
                sh '''
                    cp -r build/* /var/www/html/
                    sudo systemctl restart apache2
                '''
                echo "✅ Deployment completed successfully!"
            }
        }
    }

    post {
        failure {
            echo "❌ Pipeline failed — Check Jenkins logs!"
        }
        success {
            echo "✅ Pipeline executed successfully!"
        }
    }
}
