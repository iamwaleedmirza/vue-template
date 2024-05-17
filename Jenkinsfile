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

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Deploy') {
            environment {
                SSH_KEY = credentials('-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAtm7TMdHulLl+EqOkuzID96F1+aMb4Vixk1XHX58l1xOWpKoH
TUj7W99ypmanEqOeHNKXm4mFXGhhb91NMYwwdJyPLercY1+VEH+UhEIQ8nwKasFJ
5AIVw2ojUBURXXEYyuVi6SugmIBWK3Ts65BGnnpADbZek7aQ0MIcc9BhduFP9CXs
tUw7ZmBsd8TLPLxKV+eWaDfTyAhZYLgwM75QonMnG7ah8mBX0RRsb/0YSmLnh1W7
NnibFS8xeBHHUc/L/o4as291DxR+sjC/uldMd3XePQWlzZNbR04d3tgfH86Gchxp
nGEIWtOtuDjJ9S7mv4yNFhD2kKYjWijyDfhsvwIDAQABAoIBAQCYwlIMDIqndG9R
8j81FJtn1oZwA5mL53XCNnic5CmOX/OrbEhy+aKoeoGJPrm1jNYPsnOKpOvLxgeY
4QkLbSUB8DqgK2Jhv56VUw8sdBm2whEC9VkHGIvattxc6VdCFDq7YrqhHov6RrRg
7SaZ1ZScjSdnlAa7z4qRk4i5nYqPPIwZYK6GkgnAqVWdwc604CJjf3ny7yaT7moL
x9dgAXx0evAbs6kvNg3+QoLNbnPwucbrAmE8PKaCtB8DsNrU0/3igpCtiXI58yF7
KTdi4LAotRUMuG9FXjqhQSj7Y8wPo8vmKe+QQrZegqAf7SO8/2i5xsBTMmthUoM+
8QtRq7e5AoGBAPf+dMhyMX4/okApnQIq1DMlAblNNr4RZzqR7FH6FCJ38t81kiZJ
vU2iVr5OfKDf3/ule18f21sT15y/Kt5pycVNiIW73d/6nkm/4JwQfNTRtLccXGzd
KiXc5w4UImXVeyLWzqjl5g52w/FCXsSofqAyGbUD0ZDWI65o28GvcUGLAoGBALxS
ikALqcjEvsmc0tkXSTueuY7tiMNop4CMKY8K6bqrr76H8/jziWGmQo6g5+WJZRct
zCPFFI7Z3mjSL9D4yR8cPVPX71SyXDm9ozS91UjPK0cOPJzI2E9N027EqkKEmL2l
RNWpjWxV8pYgKzPESxMpio0ModgpYyXB0x6G7AAdAoGAQyyRxZno/iGOeYLMHMIt
KI1loiPgKCveombUdIAg5BVJnFyOcgCXXmgSxwnLiGgb67YvbNzcNhdx1+uQWR6e
oOsXh+ITf5ALQD7RQHTW227SXKc6AeMGf6sOiym5B4yEBjPZVravUkupV7h6oxpg
8lOf0wBty6W1lJCithwnuXECgYA96bz99VeqY9R9oAtJx/gRm8tRjBJMfBmlj28S
Ufopnsw2jafODvL5oZl8HrZepl8P0cStddueY05Vk9SYlVI54iTfbbyHUeQ0L356
lnaKa2HFCI8w1G8ZE3MRlaKMH9+/aZhJzmZqWY9Zf9X0PPKZqCye1qpW0LSB80kf
Xig0mQKBgQDq+YtySSGJMPgp93W4johiE9mV5tX9sNgSV29Dp5ceZKy/ImbgLF78
DlkG4K9PROxLG0sBL9NU6OfubgqDOGtXJ215GFrdlIcvvDQ9YkpmBfMIhjcSCEJx
WEB15g+eSmu4WlxQlsE3smZiXGu6GH08woAaSFzJLFKJwy/IXJIMzA==
-----END RSA PRIVATE KEY-----') // Jenkins SSH key credential ID
                EC2_INSTANCE = '50.18.136.33' // EC2 instance IP or hostname
                REMOTE_DIR = '/var/www/vue-template' // Remote directory on the EC2 instance
            }
            steps {
                // Copy files to the EC2 instance
                sh "scp -i \${SSH_KEY} -r ./\* ubuntu@\${EC2_INSTANCE}:\${REMOTE_DIR}"

                // Connect to the EC2 instance via SSH
                // sh "ssh -i \${SSH_KEY} ubuntu@\${EC2_INSTANCE} 'cd \${REMOTE_DIR} && pm2 stop app || true'"

                // Install project dependencies on the EC2 instance
                sh "ssh -i \${SSH_KEY} ubuntu@\${EC2_INSTANCE} 'cd \${REMOTE_DIR} && npm install'"

                // Start the application on the EC2 instance
               // sh "ssh -i \${SSH_KEY} ubuntu@\${EC2_INSTANCE} 'cd \${REMOTE_DIR} && pm2 start app'"
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
