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
                withCredentials([sshUserPrivateKey(credentialsId: 'test_id', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                    sh '''
                        chmod 600 "$SSH_KEY"
                        scp -i "$SSH_KEY" main "$SSH_USER"@target:~/
                    '''
                }
            }
        }
    }
}
