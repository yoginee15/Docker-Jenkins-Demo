pipeline {

  environment {

    registry = "sankethshinde/demoproject"

    registryCredential = 'docker-creds'

    dockerImage = ''

  }

  agent any

  stages {

    stage('Cloning Git') {

      steps {

        git 'https://github.com/shindesanket/Docker-Jenkins-Demo'

      }

    }

    stage('Building image') {

      steps{

        script {

          dockerImage = docker.build registry + ":$BUILD_NUMBER"

        }

      }

    }

    stage('Deploy Image') {

      steps{

        script {

          docker.withRegistry( '', registryCredential ) {

            dockerImage.push()

          }

        }

      }

    }
	  
	stage('Remove Existing Container') {

      steps{
	    PORT = 8000
		containerId = sh "docker container ls --format="{{.ID}}\t{{.Ports}}" | grep ${PORT} | awk '{print $1}'"
        sh "docker rm -f $(containerId)"

      }

    }
    
    	stage('Run Docker Image in Lab') {

      steps{

        script {

		        sh "docker run -d -p 8000:8000 ${dockerImage.imageName()}"
        }

      }

    }
	  
stage('Remove Unused docker image') {

      steps{

        sh "docker rmi $registry:$BUILD_NUMBER"

      }

    }

  }

}
