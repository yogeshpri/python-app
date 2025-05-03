pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Setup Python Environment') {
            steps {
                echo 'Creating virtual environment...'
                sh 'python3 -m venv ${VENV_DIR}'
                sh './${VENV_DIR}/bin/pip install --upgrade pip'
                sh './${VENV_DIR}/bin/pip install -r requirements.txt'
            }
        }

        stage('Run Tests with Pytest') {
            steps {
                echo 'Running tests using pytest...'
                sh './${VENV_DIR}/bin/pytest --maxfail=5 --disable-warnings'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                echo 'Generating code coverage report...'
                sh './${VENV_DIR}/bin/coverage run -m pytest'
                sh './${VENV_DIR}/bin/coverage report'
                sh './${VENV_DIR}/bin/coverage html'
            }
        }

        stage('Archive Coverage Report') {
            steps {
                echo 'Archiving HTML coverage report...'
                publishHTML(target: [
                    reportDir: 'htmlcov',
                    reportFiles: 'index.html',
                    reportName: 'Code Coverage Report'
                ])
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'rm -rf ${VENV_DIR}'
        }
    }
}

