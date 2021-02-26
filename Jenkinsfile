pipeline {
	agent any
	stages {
		stage("Deliver to Docker Hub") {
			steps {
				sh "docker build . -t lechampdk/FrontendCalculatorG2"
				withCredentials([[$class: 'UsernamePasswordMultiBinding', crendentialId: '69e5c2f8-8f3b-4461-b829-e3532bc4b156', UsernameVariable: 'USERNAME', PasswordVariable: 'PASSWORD']])
				{
					sh 'docker login -u ${USERNAME} -p ${PASSWORD}'
				}
				sh "docker push lechampdk/FrontendCalculatorG2"
			}
		}
	}
}