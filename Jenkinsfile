pipeline {
    agent {
        docker {
            image 'python:3.9'
        }
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
    }
}
