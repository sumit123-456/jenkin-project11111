pipeline {
    agent any

    tools {
        maven 'MAVEN_3'
        jdk 'JDK_17'
    }

    environment {
        IMAGE_NAME = "jenkins-demo-app"
        CONTAINER_NAME = "demo-app"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                docker run -d --name $CONTAINER_NAME -p 8080:8080 $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
