pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                sh '''
                # Create a virtual environment
                python3 -m venv venv
                # Activate the virtual environment
                source venv/bin/activate
                # Install the dependencies
                pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'pytest tests/'
            }
        }

        stage('Deploy') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                echo 'Deploying application...'
                sh 'nohup python3 app.py &'
            }
        }
    }

    post {
        success {
            mail to: 'shiv.rv007@gmail.com, salmanmuneeb@gmail.com',
                 subject: "Jenkins Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Good news! The Jenkins pipeline has successfully completed. View the details at: ${env.BUILD_URL}"
        }
        failure {
            mail to: 'shiv.rv007@gmail.com, salmanmuneeb@gmail.com',
                 subject: "Jenkins Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Oops! The Jenkins pipeline has failed. Check the logs at: ${env.BUILD_URL}"
        }
    }
}
