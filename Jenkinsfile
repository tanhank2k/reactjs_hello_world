pipeline {
    agent {node { label 'nodejs-slave' }}
    environment {
        APP_NAME = 'reactjs_hello_world'
        DOCKER_IMAGE = "tanhank2k1/${APP_NAME}:latest"
    }
    stages {
        stage('Checkout') {
            steps {
                // Check out the repository
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                // Install the necessary npm packages
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                // Run tests (assuming there are tests configured in package.json)
                sh 'npm test'
            }
        }
        stage('Build Application') {
            steps {
                // Build the React.js application for production
                sh 'npm run build'
            }
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    // Create a Docker image for the app
                    sh "docker build -t ${DOCKER_IMAGE} ."
                    
                    // // Login to Docker Hub (replace credentials with your credentials or Jenkins credentials)
                    // withCredentials([usernamePassword(credentialsId: 'eddabcf8-673d-4395-a9d1-14077f64aa08', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    //     sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                    // }
                    
                    // // Push the image to Docker Hub
                    // sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Example deployment: Run the Docker container
                    sh "docker run -d -p 3000:80 --name ${APP_NAME} ${DOCKER_IMAGE}"
                }
            }
        }
    }
    post {
        // always {
        //     // Clean up Docker container and images if needed
        //     sh "docker rm -f ${APP_NAME} || true"
        //     sh "docker rmi ${DOCKER_IMAGE} || true"
        // }
        success {
            echo 'The pipeline has completed successfully!'
        }
        failure {
            echo 'The pipeline has failed. Please check the logs.'
        }
    }
}
