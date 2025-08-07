pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-ci-cd-demo"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/Syed-Amjad/flask-ci-cd.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    . venv/bin/activate
                    pytest test_app.py
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Success Message') {
            steps {
                echo "Build and test successful! Image ${IMAGE_NAME} created."
            }
        }
    }

    post {
        failure {
            mail to: 'youremail@example.com',
                 subject: "Jenkins Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Something is wrong with ${env.JOB_NAME} #${env.BUILD_NUMBER}.\n\nCheck Jenkins for details: ${env.BUILD_URL}"
        }

        success {
            echo "Everything passed. Great job!"
        }
    }
}
