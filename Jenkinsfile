stage('Validate') {
    steps {
        sh '''
            echo "Current directory:"
            pwd

            echo "Files in workspace:"
            ls -la

            test -s index.html
            test -s style.css
        '''
    }
}
