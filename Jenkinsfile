pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
				bat """
				docker image build -t itaygeron2/hello-world-app:v1.0 project
                echo 'Build is done!'
				"""
            }
        }
	stage('Test') {
	    steps {
			bat """
			helm upgrade --install hello-world-app ./helm-chart --set image.tag=v1.0

			kubectl rollout status deployment/hello-world-app --timeout=120s

			kubectl logs -l app.kubernetes.io/instance=hello-world-app > app.log

			findstr /C:"Hello, World!" app.log
	        echo 'The tests have passed successfully!'
			"""
	    }
	}
	stage('Deploy') {
	    steps {
			bat """
			docker	 push docker.io/itaygeron2/hello-world-app:v1.0
	        echo 'The app is deployed!'
			"""
	    }
	}
    }
}
