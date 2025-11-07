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
// 		stage {
// 			steps {
// 				script {
				
// 				}
// 			}
// 		}
	}
}
