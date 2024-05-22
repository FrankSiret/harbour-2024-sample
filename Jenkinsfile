pipeline {
    agent any

    tools {
        go 'go-1.22'
    }

    stages {
        stage('Build') {
            steps {
                sh 'go build main.go'
                sh 'ls -la'
                sh 'echo "Build done!!!"'
            }
        }

        stage('Deploy') {
            steps {
                sh 'echo "Deploying..."'

                withCredentials([sshUserPrivateKey(credentialsId: 'mykey', keyFileVariable: 'mykey', usernameVariable: 'myuser')]) {
                    sh 'ls -la'
                    // sh "echo 'mykey=${mykey}'"
                    // sh "echo 'myuser=${myuser}'"
                    // sh 'chmod 600 ./mykey'
                    sh 'scp -o StrictHostKeychecking=no -i $mykey main $myuser@192.168.105.3:'
                }
            }
        }
    }
}
