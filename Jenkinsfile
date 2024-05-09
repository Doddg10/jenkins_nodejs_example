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
                sh 'npm install'
                sh 'npm run build'
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'npm start'
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
