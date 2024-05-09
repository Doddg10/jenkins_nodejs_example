pipeline {
    agent {
        label 'agentt'
    }

    environment {
        RDS_HOSTNAME = 'terraform-20240509191907114600000006.c3ms2q4myk3q.us-east-1.rds.amazonaws.com'
        RDS_USERNAME = 'admin'
        RDS_PASSWORD = 'admin12345678'
        RDS_PORT = 3306
        REDIS_HOSTNAME = 'elasticache-cluster.ofvbyz.0001.use1.cache.amazonaws.com'
        REDIS_PORT = 6379
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
                    sh '''
                    docker build --build-arg RDS_HOSTNAME="$RDS_HOSTNAME" \
                    --build-arg RDS_USERNAME="$RDS_USERNAME" \
                    --build-arg RDS_PASSWORD="$RDS_PASSWORD" \
                    --build-arg RDS_PORT="$RDS_PORT" \
                    --build-arg REDIS_HOSTNAME="$REDIS_HOSTNAME" \
                    --build-arg REDIS_PORT="$REDIS_PORT" \
                    -t my-nodejs-app .
                    '''
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
