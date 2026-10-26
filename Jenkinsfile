
pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate') {
            steps {
                sh '''
                    echo "Checking website files..."
                    pwd
                    ls -la
                    test -s index.html
                    test -s style.css
                    echo "Validation successful"
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    set -e
                    mkdir -p /var/www/jenkins-site
                    rsync -av \
                      --exclude='.git/' \
                      --exclude='Jenkinsfile' \
                      --exclude='README.md' \
                      ./ /var/www/jenkins-site/
                    curl -fsS http://127.0.0.1/ >/dev/null
                '''
            }
        }
    }

    post {
        success {
            echo 'Website deployed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
