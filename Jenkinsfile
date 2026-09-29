pipeline {
	agent any

	stages {
		stage('Checkout') {
			steps {
				checkout scm
			}
		}

		stage('Docker Build') {
			steps {
				script {
					if (isUnix()) {
						sh 'docker build -t devops-demo .'
					} else {
						bat 'docker build -t devops-demo .'
					}
				}
			}
		}

		stage('Test') {
			steps {
				script {
					if (isUnix()) {
						sh '''docker run --rm devops-demo python -c "import app; assert app.home() == 'Hello from DevOps Pipeline!'"'''
					} else {
						bat '''docker run --rm devops-demo python -c "import app; assert app.home() == 'Hello from DevOps Pipeline!'"'''
					}
				}
			}
		}

		stage('Deploy') {
			steps {
				script {
					if (isUnix()) {
						sh 'docker rm -f devops-container || true'
						sh 'docker run -d --name devops-container -p 5000:5000 devops-demo'
					} else {
						bat 'docker rm -f devops-container 2>NUL || exit /b 0'
						bat 'docker run -d --name devops-container -p 5000:5000 devops-demo'
					}
				}
			}
		}
	}

	post {
		success {
			echo 'Pipeline executed successfully!'
		}
		failure {
			echo 'Pipeline execution failed.'
		}
	}
}
