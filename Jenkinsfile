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
		stage("Selenium grid setup") {
			steps {
				sh "docker network create SEG2"
				sh "docker run -d --rm -p 22222:4444 --net=SEG2 --name selenium-hubg2 selenium/hub"
				sh "docker run -d --rm --net=SEG2 -e HUB_HOST=selenium-hubg2 --name selenium-node-firefoxg2 selenium/node-firefox"
				sh "docker run -d --rm --net=SEG2 -e HUB_HOST=selenium-hubg2 --name selenium-node-chromeg2 selenium/node-chrome"
				sh "docker run -d --rm --net=SEG2 --name frontendcalculatorgroup2 lechampdk/frontendcalculatorgroup2"
			}
		}
		stage("Execute system tests") {
			steps {
				sh "selenium-side-runner --server http://localhost:22222/wd/hub -c 'browserName=firefox' --base-url http://frontendcalculatorgroup2 Test/System/FunctionalTest.side"
				sh "selenium-side-runner --server http://localhost:22222/wd/hub -c 'browserName=chrome' --base-url http://frontendcalculatorgroup2 Test/System/FunctionalTest.side"
			}
		}
	}
	post {
		cleanup {
			echo "Cleaning the Docker environment"
			sh script:"docker stop selenium-hubg2", returnStatus:true
			sh script:"docker stop selenium-node-firefoxg2", returnStatus:true
			sh script:"docker stop selenium-node-chromeg2", returnStatus:true
			sh script:"docker stop frontendcalculatorgroup2", returnStatus:true
			sh script:"docker network remove SEG2", returnStatus:true
		}
	}
}