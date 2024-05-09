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
                    // Use withCredentials block to access the credentials
                    withCredentials([
                        string(credentialsId: 'rds_hostname', variable: 'RDS_HOSTNAME'),
                        string(credentialsId: 'rds-username', variable: 'RDS_USERNAME'),
                        string(credentialsId: 'rds-password', variable: 'RDS_PASSWORD'),
                        string(credentialsId: 'rds-port', variable: 'RDS_PORT'),
                        string(credentialsId: 'redis-hostname', variable: 'REDIS_HOSTNAME'),
                        string(credentialsId: 'redis-port', variable: 'REDIS_PORT')
                    ]) {
                        sh '''
                        docker build --build-arg RDS_HOSTNAME=$RDS_HOSTNAME \
                        --build-arg RDS_USERNAME=$RDS_USERNAME \
                        --build-arg RDS_PASSWORD=$RDS_PASSWORD \
                        --build-arg RDS_PORT=$RDS_PORT \
                        --build-arg REDIS_HOSTNAME=$REDIS_HOSTNAME \
                        --build-arg REDIS_PORT=$REDIS_PORT \
                        -t my-nodejs-app .
                        '''
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    sh 'docker run -d -p 3000:3000 my-nodejs-app'
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
