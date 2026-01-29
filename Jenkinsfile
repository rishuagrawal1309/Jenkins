pipeline {
    agent any

    stages {

        stage('Setup venv') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                if [ -f requirements.txt ]; then
                    pip install -r requirements.txt
                fi
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                . venv/bin/activate
                if [ -f test_main.py ]; then
                    pip install pytest
                    pytest
                else
                    echo "No tests found, skipping test stage"
                fi
                '''
            }
        }

        stage('Run main.py') {
            steps {
                sh '''
                . venv/bin/activate
                python main.py
                '''
            }
        }
    }
}
