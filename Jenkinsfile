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

        stage('Staging Deploy') {
            steps {
                sh '''
                . venv/bin/activate
                echo "Deploying application to staging..."
                nohup python main.py > staging.log 2>&1 &
                echo $! > staging.pid
                '''
            }
        }

        stage('Monitor Staging') {
    steps {
        sh '''
        echo "Monitoring staging deployment..."

        if ps -p $(cat staging.pid) > /dev/null
        then
            echo "✅ Application is running in staging"
            echo "Stopping staging app..."
            kill $(cat staging.pid)
        else
            echo "❌ Application is NOT running"
            exit 1
        fi
        '''
    }
}

        }
    }
}
