pipeline {
    agent any

    environment {
        // Creates an isolated virtual environment in your assigned workspace
        VENV_PATH = "${WORKSPACE}/venv"
    }

    stages {
        stage('Build') {
            steps {
                echo 'Setting up Python Virtual Environment & Installing Dependencies...'
                sh '''
                    python3 -m venv ${VENV_PATH}
                    . ${VENV_PATH}/bin/activate
                    pip install --upgrade pip
                    if [ -f requirements.txt ]; then pip install -r requirements.txt; fi
                    pip install pytest
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running Automated Unit Tests with Pytest...'
                sh '''
                    . ${VENV_PATH}/bin/activate
                    pytest
                '''
            }
        }

        stage('Deploy to Staging') {
            when {
                branch 'jenkins-pipeline' // Target your feature branch
            }
            steps {
                echo 'Deploying Application to Staging Environment...'
                sh '''
                    . ${VENV_PATH}/bin/activate
                    echo "Application successfully deployed to mock staging environment."
                '''
            }
        }
    }
}