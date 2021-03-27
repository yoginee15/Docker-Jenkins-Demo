pipeline {

  environment {

    registry = "sankethshinde/demoproject"

    registryCredential = 'docker-creds'

    dockerImage = ''
	  
    command = "docker container ls --format="{{.ID}}\t{{.Ports}}" | grep "8000" | awk '{print $1}"

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
	      sh "docker rm -f ${command}"
      }

    }
    
    	stage('Run Docker Image in Lab') {

      steps{

        script {

		        sh "docker run -d -p 8000:8000 ${dockerImage.imageName()}"
        }

      }

	}

  }

}
