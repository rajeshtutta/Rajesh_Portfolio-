pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "rajeshtutta123/rajesh_portfolio"
        AWS_CREDS = credentials('aws-creds')
        AWS_DEFAULT_REGION = 'us-east-1'
        RECIPIENTS = 'rajeshtutta123@gmail.com'
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/rajeshtutta/Rajesh_Portfolio-.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $DOCKER_IMAGE:latest'
                    sh 'docker logout'
                }
            }
        }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
    }

    post {
        success {
            emailext (
                subject: "Jenkins Job '${env.JOB_NAME}' Success",
                body: "Good news! Job '${env.JOB_NAME}' (#${env.BUILD_NUMBER}) succeeded.\n\nCheck console output at ${env.BUILD_URL}",
                to: "${RECIPIENTS}"
            )
        }
        failure {
            emailext (
                subject: "Jenkins Job '${env.JOB_NAME}' Failed",
                body: "Alert! Job '${env.JOB_NAME}' (#${env.BUILD_NUMBER}) failed.\n\nCheck console output at ${env.BUILD_URL}",
                to: "${RECIPIENTS}"
            )
        }
    }
}
        stage('Build') {
    steps {
        echo "Building..."
    }
    post {
        success {
            emailext(
                subject: "Build Stage Success",
                body: "Build completed successfully",
                to: "${RECIPIENTS}"
            )
        }
    }
}

        stage('Deploy to EKS') {
            steps {
                sh '''
                export AWS_ACCESS_KEY_ID=$AWS_CREDS_USR
                export AWS_SECRET_ACCESS_KEY=$AWS_CREDS_PSW

                aws eks update-kubeconfig --region us-east-1 --name mycluster1
                kubectl apply -f deployment.yml
                kubectl apply -f service.yml
                '''
            }
        }

    }
}
