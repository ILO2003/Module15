pipeline {
    agent any
    stages {
        stage("copy files to ansible server") {
            steps {
                script {
                    echo "copying files to ansible control node"
                    sshagent(['ansible-server-key']){
                        sh "scp -o StrictHostKeyChecking=no ansible/* root@167.23.86.20:/root"

                        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable:'keyfile', usernameVariable: 'user')]) {
                            sh "scp ${keyile} root@167.23.86.20:/root/ssh-key.pem
                        }
                    }
                }
            }
        }
    }
}
