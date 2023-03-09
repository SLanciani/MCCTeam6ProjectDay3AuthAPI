node {
    stage ("Checkout AuthService"){
        git branch: 'main', url: ' https://github.com/SLanciani/MCCTeam6ProjectDay3AuthAPI.git'
    }
    
    stage ("Gradle Build - AuthService") {
        sh 'gradle clean build'
    }
    
    stage ("Gradle Bootjar-Package - AuthService") {
        sh 'gradle bootjar'
    }
    
    	stage ("Containerize the app-docker build - AuthApi") {
        sh 'docker build --rm -t mcc-auth:v1.0 .'
    }
    
    stage ("Inspect the docker image - AuthApi"){
        sh "docker images mcc-auth:v1.0"
        sh "docker inspect mcc-auth:v1.0"
    }
    
    stage ("Run Docker container instance - AuthApi"){
        sh "docker run -d --rm --name mcc-auth -p 8081:8081 mcc-auth:v1.0"
     }
     
    stage('User Acceptance Test - AuthService') {
	
	  def response= input message: 'Is this build good to go?',
	   parameters: [choice(choices: 'Yes\nNo', 
	   description: '', name: 'Pass')]
	
	  if(response=="Yes") {
		  
	stage('Deploy to Kubenetes cluster - AuthApi') {
		  sh "docker stop mcc-auth"
	      sh "kubectl create deployment mcc-auth --image=mcc-auth:v1.0"
		 //get the value of API_HOST from kubernetes services and set the env variable
	      sh "set env deployment/mcc-auth API_HOST=\$(kubectl get service/mcc-data -o jsonpath='{.spec.clusterIP}'):8080"
	      sh "kubectl expose deployment mcc-auth --type=LoadBalancer --port=8081"
	    }
	   
	  }
    }
	
	stage("Production Deployment View"){
        sh "kubectl get deployments"
        sh "kubectl get pods"
        sh "kubectl get services"
    }
}
