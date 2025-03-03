pipeline {
    agent any

    environment {
        DEPLOY_PATH = "/var/www/html/protouch"  // Apache deployment directory
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/ankit-kumar87/protouch-ci-cd'
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    echo "Running HTML & CSS lint checks..."
                    sh '''
                    # Install linters if not available (only needed once)
                    sudo apt-get install tidy -y
                    sudo apt-get install csslint -y

                    # Run HTML linter
                    tidy -errors index.html || true

                    # Run CSS linter
                    for file in css/*.css; do csslint "$file" || true; done
                    '''
                }
            }
        }

        stage('Deploy to Apache') {
            steps {
                script {
                    echo "Deploying website to Apache..."
                    sh '''
                    sudo rm -rf $DEPLOY_PATH
                    sudo mkdir -p $DEPLOY_PATH
                    sudo cp -r * $DEPLOY_PATH
                    sudo chown -R www-data:www-data $DEPLOY_PATH
                    sudo chmod -R 755 $DEPLOY_PATH
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "Deployment Successful! Website is live on Apache."
        }
        failure {
            echo "Build or Deployment Failed!"
        }
    }
}
