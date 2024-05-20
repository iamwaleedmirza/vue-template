pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'laravel', keyFileVariable: 'SSH_KEY')]) {
                    script {
                        def sshScript = '''
                            # Copy files to the EC2 instance
ssh -o StrictHostKeyChecking=no -i "$SSH_KEY" ubuntu@50.18.136.3 'mkdir -p /var/www/html2'
                            #scp -i "$SSH_KEY" -r ./* ubuntu@50.18.136.33:~/var/www/html
                            #ssh -o StrictHostKeyChecking=no -i "$SSH_KEY" ubuntu@50.18.136.33 'sudo apt update'
                            # Connect to the EC2 instance via SSH and execute commands
                          #  ssh -o StrictHostKeyChecking=no -i "$SSH_KEY" ubuntu@50.18.136.33 'cd ~/var/www/html && npm install && npm start'
                        '''
                     //   sshScript = sshScript.replace("50.18.136.33", env.50.18.136.33)
                     //   sshScript = sshScript.replace("/var/www/html", env./var/www/html)
                      //  sh sshScript
                    }
                }
            }
        }
    }
}
