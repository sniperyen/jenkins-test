pipeline {
    agent none
    stages {
        stage('Build') {
            steps {
                echo 'make package'
            }
        }
        stage('Test') {
            steps {
                echo 'make check'
            }
        }
        stage('Deploy') {
            when { tag "*" }
            steps {
                echo 'make deploy'
            }
        }
    }
}
