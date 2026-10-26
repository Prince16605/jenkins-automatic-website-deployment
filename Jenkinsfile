
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate') {
            steps {
                sh '''
                    test -s index.html
                    test -s style.css
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    rsync -av --delete \
                      --exclude='.git/' \
                      --exclude='Jenkinsfile' \
                      --exclude='README.md' \
                      ./ /var/www/jenkins-site/

                    curl -fsS http://127.0.0.1/ >/dev/null
                '''
            }
        }
    }
}
