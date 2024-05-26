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
                    sh "scp -o StrictHostKeychecking=no -i ${mykey} main ${myuser}@192.168.105.3:"
                }
            }
        }

        stage('Run service') {
            steps {
                sh 'echo "Running service..."'

                withCredentials([sshUserPrivateKey(credentialsId: 'mykey', keyFileVariable: 'mykey', usernameVariable: 'myuser')]) {
                    sh "ssh -i ${mykey} ${myuser}@192.168.105.3 \"sudo systemctl stop myapp\""
                    sh "scp -o StrictHostKeychecking=no -i ${mykey} myapp.service ${myuser}@192.168.105.3:"

                    sh "ssh -i ${mykey} ${myuser}@192.168.105.3 \"sudo mv myapp.service /etc/systemd/system/\""
                    sh "ssh -i ${mykey} ${myuser}@192.168.105.3 \"sudo systemctl daemon-reload\""
                    sh "ssh -i ${mykey} ${myuser}@192.168.105.3 \"sudo systemctl start myapp\""
                    sh "ssh -i ${mykey} ${myuser}@192.168.105.3 \"sudo systemctl status myapp\""
                    sh "ssh -i ${mykey} ${myuser}@192.168.105.3 \"sudo systemctl enable myapp\""
                }
            }
        }
    }
}
