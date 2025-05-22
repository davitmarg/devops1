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
                sh "go build -o main main.go"
            }
        }
        stage('Deploy') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'test-id', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                    sh '''
                        ssh-keyscan -H target >> ~/.ssh/known_hosts
                        scp -i "$SSH_KEY" main "$SSH_USER"@target:~/
                    '''
                }
            }
        }
    }
}
