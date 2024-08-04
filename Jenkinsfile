pipeline {
    agent any
    stages {
        stage("copy files to ansible server") {
            steps {
                script {
                    echo "copying files to ansible control node"
                    sshagent(['ansible-server-key']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* root@167.23.86.20:/root"

                        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                            sh 'scp keyfile root@167.23.86.20:/root/ssh-key.pem'
                        }
                    }
                }
            }
        }
        stage ("execute ansible playbook") {
            steps{
                script{
                    echo "calling ansible playbook to configure servers"
                    def remote = [:]
                    remote.name = "ansible-server"
                    remote.host = "167.23.86.20"
                    remote.allowAnyHosts = true

                    withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                            remote.user = user
                            remote.identityFile keyfile
                            sshCommand remote: remote, command: "ansible-playbook my-playbook.yaml"
                        } 
                }
            }
        }
    }
}
