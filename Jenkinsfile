pipeline {
    agent any

    tools {
       go "1.24.1"
    }

    stages {
        stage('Test') {
                steps {
                    sh "go test ./..."
                }
            }
        stage('Build') {
            steps {
                sh "go build main.go"
            }
        }
        stage('Deploy') {
            steps {
                sh 'mkdir -p ~/.ssh'
                sh 'ssh-keyscan target >> ~/.ssh/known_hosts'
                sh 'scp main laborant@target:~'
            }
        }
    }
}
