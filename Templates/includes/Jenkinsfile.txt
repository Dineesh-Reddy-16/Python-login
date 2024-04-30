#!/groovy
def dockerImageRepo = 'gatewaytech/gatewaytech-ui'
def dockerImageTag
def dockerImage
def dockerRegistry = 'hub.docker.com'
def bu_nu

pipeline
{
	agent any
	stages
	{
		stage('Cleaning the WorkSpace')
		{
			steps
			{
				deleteDir()
				echo "the build number is ${currentBuild.number}"
				echo 'Cleanup Done'
				script 
				{

					dockerImageTag="$dockerImageRepo"+":"+"$BUILD_NUMBER"
					echo "Created a Tag for uploading an Image to Registry based on Build_Number : $dockerImageTag"
				}
			}
		}


		stage('CheckOut latest Code')
		{
			steps
			{
				checkout scm
				script 
				{

					dockerImageTag="$dockerImageRepo"+":"+"$BUILD_NUMBER"
					echo "Created a Tag for uploading an Image to Registry based on Build_Number : $dockerImageTag"
				}
			}
		}

		stage('Build the Image')
		{
			steps
			{
				script 
				{
					echo 'Starting the Image Building'
					dockerImage = docker.build "${dockerImageTag}"
					sh 'docker images'
					sh 'docker ps -a'
					echo "$dockerImage"
				}
			}
		}

		stage('Publish Docker Images to DockerHub')
		{
			steps
			{
				echo "Pushing Docker image to Registory"
				script
				{
					sh 'docker login --username="anandgit71" --password="anandgit12" ${dockerRegistry}'
					dockerImage.push()
					sh 'docker images'
				}
			}
		}
		
		stage('Deploy the Flask App')
		{
			steps
			{
				script
				{
					sh 'docker rm -f Flask-App'
					sh 'docker run -t -d -p 5000:5000 --name Flask-App gatewaytech/gatewaytech-ui:$BUILD_NUMBER'
				}
			}
		}
	}
}

