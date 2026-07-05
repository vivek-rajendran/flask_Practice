pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Installing application dependencies directly into the workspace...'
                sh '''
                    # Upgrade pip and install requirements directly
                    python3 -m pip install --upgrade pip --user || echo "Proceeding with default pip"
                    
                    if [ -f requirements.txt ]; then 
                        python3 -m pip install -r requirements.txt --user --break-system-packages || python3 -m pip install -r requirements.txt --user || pip install -r requirements.txt
                    fi
                    
                    # Ensure pytest is installed for the next stage
                    python3 -m pip install pytest --user --break-system-packages || python3 -m pip install pytest --user || pip install pytest
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Executing pytest automated suites...'
                sh '''
                    # Try executing pytest from path, or fallback to running it as a python module
                    pytest || python3 -m pytest
                '''
            }
        }

        stage('Deploy to Staging') {
            when {
                branch 'jenkins-pipeline'
            }
            steps {
                echo 'Simulating verification checks and running staging deployment logs...'
                sh 'echo "Application seamlessly routed to staging target environment."'
            }
        }
    }
}