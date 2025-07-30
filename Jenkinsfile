pipeline {
    agent any
    environment {
        PIP_USER = "1"
        PATH = "${HOME}/.local/bin:${PATH}"
    }	
    stages {
        stage('Build') { 
            steps {
			    sh 'whoami'
                sh 'python3 -m py_compile sources/add2vals.py sources/calc.py' 
                stash(name: 'compiled-results', includes: 'sources/*.py*') 
            }
        }
		stage('Test') {
            steps {
			    sh 'mkdir -p test-reports'
                sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
    }
}
