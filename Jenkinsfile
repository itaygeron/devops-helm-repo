pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
				bat """
				docker image build -t itaygeron2/hello-world-app:v1.0 project
				"""
				echo 'Build is done!'
            }
        }
	stage('Test') {
	    steps {
			bat """
			helm upgrade --install hello-world-app ./hello-world-app --set image.tag=v1.0

			kubectl rollout status deployment/hello-world-app --timeout=120s

			powershell -NoProfile -Command "\$response = Invoke-WebRequest -Uri 'http://localhost:30007' -UseBasicParsing; \$body = \$response.Content; Write-Host 'Response:'; Write-Host \$body; if (\$body -notlike '*Hello, World!*') { Write-Error 'Expected Hello, World! not found on localhost:30007'; exit 1 }"
			"""
			echo 'The tests have passed successfully!'
	    }
	}
	stage('Deploy') {
	    steps {
	        withCredentials([usernamePassword(
	            credentialsId: 'dockerhub-creds',
	            usernameVariable: 'DOCKER_USER',
	            passwordVariable: 'DOCKER_PASS'
	        )]) {
	            bat '''
	            echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
	
	            docker build -f Project\\Dockerfile -t %DOCKER_USER%/hello-world-app:v1.0 Project
	
	            docker push %DOCKER_USER%/hello-world-app:v1.0
	
	            docker logout
	            '''
				echo 'The app is successfully deployed!'
	        }
	    }
    }
}
