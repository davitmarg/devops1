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
                sh '''
                    go build -o main main.go
                '''
            }
        }

        stage('Docker') {
            steps {
                sh '''
                    docker build -t myapp .

                    docker tag myapp ttl.sh/myapp:2h
                    docker push ttl.sh/myapp:2h

                '''
            }
        }

         stage('Deploy') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'test-id', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                    sh '''
                        mkdir -p ~/.ssh
                        chmod 700 ~/.ssh
                        ssh-keyscan -H 3.72.14.87 >> ~/.ssh/known_hosts

                        ssh -i "$SSH_KEY" ubuntu@3.72.14.87 '

                            docker pull ttl.sh/myapp:2h
                            docker stop myapp || true
                            docker rm myapp || true
                            docker run -d --name myapp -p 4444:4444 ttl.sh/myapp:2h
                        '
                    '''
                }
            }
        }
    }
}
