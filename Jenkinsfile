pipeline {
    agent any
    environment {
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
			    // install command to 
			    //sh 'pip install pytest'
                sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
		stage('Deliver') {
            steps {
			    sh "pip install pyinstaller"
                sh "pyinstaller --onefile sources/add2vals.py"
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}
