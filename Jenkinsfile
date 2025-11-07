pipeline{
	agent any
// 	{
//         docker {
//             // Use the custom image you built
//             image 'jenkins/jenkins:lts-jdk17'
//             // Explicitly mount the socket for safety, though often automatic
//             args '-v /var/run/docker.sock:/var/run/docker.sock' 
//             // Optional: If you want to use the agent node's workspace directly
//             // reuseNode true 
//         }
//     }
	environment {
		DOCKERHUB_CREDS = credentials('docker-id')
		IMAGE_NAME = "9767264150/flaskapp"
		GITHUB_USER = "sumit-9767"
		GITHUB_REPO = "two-tier-app"
	}
	stages{
		stage ('Checkout'){
			steps {
				git branch: 'main', url: 'https://github.com/sumit-9767/two-tier-app.git'
			}
		}
		stage ('Build'){
			steps {
				script {
				   docker.build("${IMAGE_NAME}:${BUILD_ID}")
				}
			}
		}
		stage ('Login and Push Image'){
			steps {
				script {
				    sh ' echo "${DOCKERHUB_CREDS_PSW}" | docker login -u "${DOCKERHUB_CREDS_USR}" --password-stdin '
				    sh 'docker push ${IMAGE_NAME}:${BUILD_ID}'
				}
			}
		}
		stage ('Deploy to K8s') {
			steps {
				script {
				    withCredentials([string(credentialsId: 'github', variable: 'GIT_TOKEN')]) {
    				    sh '''
    				        git config user.email "batras781@gmail.com"
    				        git config user.name "sumit"
    				        sed -i "s|image: 9767264150/flaskapp:.*|image: 9767264150/flaskapp:${BUILD_ID}|g" two_deploy.yaml
    				        git add two_deploy.yaml
    				        git commit -m "Updating the version of image to ${BUILD_ID}"
    				        git push https://${GIT_TOKEN}@github.com/sumit-9767/two-tier-app/ HEAD:main
    				    '''
				    }
				
				}
			}
		}
	}
}


