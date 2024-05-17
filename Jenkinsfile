pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/iamwaleedmirza/vue-template.git']]])
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([string(credentialsId: 'VUEKEY', variable: 'SSH_KEY')]) {
                    // Retrieve the environment variables from GitHub Secrets
                    def ec2Instance = env.EC2_INSTANCE
                    def remoteDir = env.REMOTE_DIR

                    // Copy files to the EC2 instance
                    sh "scp -i $VUEKEY -r ./* ubuntu@$EC2_INSTANCE:$REMOTE_DIR"

                    // Connect to the EC2 instance via SSH
                    // sh "ssh -i $SSH_KEY ubuntu@$EC2_INSTANCE 'cd $REMOTE_DIR && pm2 stop app || true'"

                    // Install project dependencies on the EC2 instance
                    sh "ssh -i $VUEKEY ubuntu@$EC2_INSTANCE 'cd $REMOTE_DIR && npm install'"

                    // Start the application on the EC2 instance
                    // sh "ssh -i $SSH_KEY ubuntu@$ec2Instance 'cd $remoteDir && pm2 start app'"
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
