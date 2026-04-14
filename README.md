groovy

pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Pulls code from Git
                git branch: 'main', url: 'https://github.com/your-repo/mind-track.git'
            }
        }
        stage('Build & Train') {
            steps {
                // Installs dependencies and trains the model
                sh 'pip install -r requirements.txt'
                sh 'python train_model.py'
            }
        }
        stage('Validate/Track') {
            steps {
                // Runs evaluation script to track accuracy
                sh 'python validate_model.py'
                // Publish results (e.g., HTML report or API call to dashboard)
                publishHTML(target: [reportFiles: 'report.html'])
            }
        }
        stage('Deploy') {
            steps {
                // Deploy the new model to production/serving
                sh 'docker build -t mind-track-app .'
                sh 'docker push your-registry/mind-track-app'
            }
        }
    }
}
