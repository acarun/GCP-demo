pipeline {
    agent any
    environment {
        PROJECT_ID = 'arun-devops-265211'
        CLUSTER_NAME = 'kube-arun'
        LOCATION = 'us-central1-a'
        CREDENTIALS_ID = 'kubernetes'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
		 stage("Build") {
            steps {
               echo "cleaning and packaging"
			   sh 'mvn clean package'
            }
        }
		 stage("Test") {
            steps {
                echo "Testing"
			   sh 'mvn test'
            }
        }
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("acarunk/k8s:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
			sh 'docker image ls'
			docker.withRegistry('https://registry.hub.docker.com', 'Docker-hub-credentials') 
			  {
			    
                           myapp.push("${env.BUILD_ID}")
				
                    }
                }
            }
        }        
        stage('Deploy to GKE') {
            steps{
			    echo "Deployment started"
				sh 'ls -ltr'
				sh 'pwd'
                                    script{
	                        try{
	                            sh ‘kubectl apply -f .’
	                            sh ‘kubectl apply -f .’
	

	                          }catch(error)
			          {
	                            sh ‘kubectl create -f .’
	                            sh ‘kubectl create -f .’
	

	                          }
	                                }

            }
        }
    }    
}
