pipeline {
    agent any

    tools {
       go "1.24.1"
    }

    stages {
    stage('Test') {
            steps {
                sh "go test ./main_test.go"
            }
        }
        stage('Build') {
            steps {
                sh "go build main.go"
            }
        }
    }
}
