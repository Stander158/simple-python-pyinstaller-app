pipeline {
    agent {
        docker {
            image 'python:3.9'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Setup Environment') {
            steps {
                // Ensure required Python packages are installed
                sh '''
                pip install --upgrade pip
                pip install pytest pyinstaller
                '''
            }
        }
        stage('Build') {
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                stash(name: 'compiled-results', includes: 'sources/*.py*')
            }
        }
        stage('Test') {
            steps {
                sh '''
                py.test --junit-xml test-reports/results.xml sources/test_calc.py
                '''
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') { 
            steps {
                // Create a standalone executable using PyInstaller
                sh '''
                pyinstaller --onefile sources/add2vals.py
                '''
            }
            post {
                success {
                    // Archive the built artifact
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}
