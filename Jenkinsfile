pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    docker { image 'python:3' }
                    // image 'python:3.8-slim-buster'
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'pip --version'
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            agent {
                docker {
                    image 'cdrx/pyinstaller-linux:python3'
                }
            }
            steps {
                // sh 'virtualenv venv && . venv/bin/activate && pip3 install pyinstaller && pyinstaller --onefile sources/add2vals.py'
                sh 'pip --version'
                sh 'pip3 install pyinstaller && pyinstaller --onefile sources/add2vals.py'
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}
