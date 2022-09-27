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
            sh 'docker build . -t 058107942271.dkr.ecr.us-east-1.amazonaws.com/nodewebapp:latest'
        }
    }
	  
    stage('AWS ECR Login') {
        steps {
		script {
            sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 058107942271.dkr.ecr.us-east-1.amazonaws.com'
        }
	}
    }
    stage('Push Docker image to AWS ECR Repository') {
        steps {
		script {
            sh 'docker push 058107942271.dkr.ecr.us-east-1.amazonaws.com/nodewebapp:latest'
        }
	}
    }
    stage('Deploy Application') {
        steps {
            sh 'scp -o StrictHostKeyChecking=no -i /home/ubuntu/AssignmentMngKey.pem /var/lib/jenkins/workspace/UpgradFinalProject/deployment.sh ubuntu@10.0.30.236:~/'
            sh 'ssh -o StrictHostKeyChecking=no -i /home/ubuntu/AssignmentMngKey.pem ubuntu@10.0.30.236 "chmod +x ~/deployment.sh"'
            sh 'ssh -o StrictHostKeyChecking=no -i /home/ubuntu/AssignmentMngKey.pem ubuntu@10.0.30.236 "sudo ~/deployment.sh"'
	}
    }
  }
}
