pipeline {
    agent {
        label 'agentt'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'rds_redis', url: 'https://github.com/Doddg10/jenkins_nodejs_example.git'
            }
        }
        
        stage('Build') {
            steps {
                script {
                    // Build the Docker image without passing environment variables
                    sh 'docker build -t my-nodejs-app .'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    withCredentials([
                        string(credentialsId: 'rds_hostname', variable: 'RDS_HOSTNAME'),
                        string(credentialsId: 'rds-username', variable: 'RDS_USERNAME'),
                        string(credentialsId: 'rds-password', variable: 'RDS_PASSWORD'),
                        string(credentialsId: 'rds-port', variable: 'RDS_PORT'),
                        string(credentialsId: 'redis-hostname', variable: 'REDIS_HOSTNAME'),
                        string(credentialsId: 'redis-port', variable: 'REDIS_PORT')
                    ]) {
                        sh '''
                        docker run -d -p 3000:3000 \
                        -e RDS_HOSTNAME=$RDS_HOSTNAME \
                        -e RDS_USERNAME=$RDS_USERNAME \
                        -e RDS_PASSWORD=$RDS_PASSWORD \
                        -e RDS_PORT=$RDS_PORT \
                        -e REDIS_HOSTNAME=$REDIS_HOSTNAME \
                        -e REDIS_PORT=$REDIS_PORT \
                        my-nodejs-app
                        '''
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Deployment successful'
        }
        failure {
            echo 'Deployment failed'
        }
    }
}
