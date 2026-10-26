
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
                    set -eu
                    echo "Workspace: $(pwd)"
                    ls -la
                    test -s index.html
                    test -s style.css
                    grep -qi '<html' index.html
                    echo "Validation passed"
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    set -eu
                    rsync -av --delete \
                      --exclude='.git/' \
                      --exclude='Jenkinsfile' \
                      --exclude='README.md' \
                      ./ /var/www/jenkins-site/
                    test -s /var/www/jenkins-site/index.html
                    curl -fsS http://127.0.0.1/ >/dev/null
                    echo "Deployment and HTTP verification passed"
                '''
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Website deployed!'
        }
        failure {
            echo 'FAILED: Check the first error in Console Output.'
        }
    }
}
