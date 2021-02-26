pipeline {
	agent any
	stages {
		stage("Deliver to Docker Hub") {
			steps {
				sh "docker build . -t lechampdk/frontendcalculatorgroup2"
				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: '69e5c2f8-8f3b-4461-b829-e3532bc4b156', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']])
				{
					sh 'docker login -u ${USERNAME} -p ${PASSWORD}'
				}
				sh "docker push lechampdk/frontendcalculatorgroup2"
			}
		}
	}
}