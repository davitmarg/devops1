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
                        docker build -t my_orz_app .

                        docker tag my_orz_app ttl.sh/my_orz_app:2h
                        docker push ttl.sh/my_orz_app:2h

                    '''
                }
            }

            stage('Deploy') {
                steps {
                    withCredentials([sshUserPrivateKey(credentialsId: 'test-id', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                        sh '''
                            mkdir -p ~/.ssh
                            chmod 700 ~/.ssh
                            ssh-keyscan -H docker >> ~/.ssh/known_hosts

                            ssh -i "$SSH_KEY" "$SSH_USER"@docker '

                                docker pull ttl.sh/my_orz_app:2h
                                docker stop my_orz_app || true
                                docker rm my_orz_app || true
                                docker run -d --name my_orz_app -p 4444:4444 ttl.sh/my_orz_app:2h
                            '
                        '''
                    }
                }
            }
        }
    }
