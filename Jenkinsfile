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
                sh 'ssh-keyscan 172.16.0.3 >> ~/.ssh/known_hosts'
                sh 'scp main laborant@172.16.0.3:~'
            }
        }
    }
}
