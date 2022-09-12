pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
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
                    image 'cdrx/pyinstaller-linux:python2'
                    args '-i --volume-from=1a6bb34b4a47e2836dd765a5eb554dcca948f453f799686b92b353a1fa37e0f8'
                }
            }
            steps {
                input message: 'Lanjutkan ke tahap Deploy? (Click "Proceed" to continue)'
                sh '/usr/local/bin/pyinstaller --onefile sources/add2vals.py'
            }
            post {
                success {
                    sh 'sleep 300'
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}
