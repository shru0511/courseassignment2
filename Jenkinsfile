pipeline {
  agent any
    
  stages {
        
    stage('Checkout code') {
        steps {
            checkout scm
        }
	}
     
    stage('Docker Image Build') {
        steps {
            sh 'docker build . -t 785734458128.dkr.ecr.us-east-1.amazonaws.com/appserver-node '
        }
    }
	  
    stage('AWS ECR Login') {
        steps {
		script {
            sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 785734458128.dkr.ecr.us-east-1.amazonaws.com'
        }
	}
    }
    stage('Push Docker image to AWS ECR Repository') {
        steps {
		script {
            sh 'docker push 785734458128.dkr.ecr.us-east-1.amazonaws.com/appserver-node:latest'
        }
	}
    }
    stage('Deploy Application') {
        steps {
            sh 'ssh -o StrictHostKeyChecking=no -i /home/ubuntu/AwsKeyPair.pem ubuntu@10.0.2.176 "chmod +x ~/deployment.sh"'
            sh 'ssh -o StrictHostKeyChecking=no -i /home/ubuntu/AwsKeyPair.pem ubuntu@10.0.2.176 "sudo ~/deployment.sh"'
	}
    }
  }
}
