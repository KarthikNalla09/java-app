pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/KarthikNalla09/java-app.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
                
                sh 'docker build -t samplewebapp:latest .' 
                sh 'docker tag samplewebapp karthiknalla09/java-app:latest'
               	                 
          }
        }
     
       
      stage('Run Docker') {
             
            steps 
			{
                sh "docker run --name java-app -d -p 8008:8080 karthiknalla09/java-app:latest"
				sh 'sleep 10'
				
				
 
            }
        }
	 
	 
	 stage('Test') {
           steps {
                
               sh 'curl localhost:8080/health'
				sh 'sleep 20'
               	                 
          }
        }
 
	 stage('Delete infra') {
           steps {
                
                sh 'docker stop java-app'
				sh 'docker rm java-app'
               	                 
          }
        }
	 
	 stage('Publish image to Docker Hub') {
          
            steps {
        
		withCredentials([string(credentialsId: 'DOCKER_USER', variable: 'DOCKER_USER'), string(credentialsId: 'PASSWD', variable: 'DOCKER_PASSWORD')]) {
            sh 'docker login -u $DOCKER_USER -p $DOCKER_PASSWORD'
              sh  'docker push ishaqmd/javaapp:latest'
          }
		  

}
        
                  
          
  }
    }
	}
