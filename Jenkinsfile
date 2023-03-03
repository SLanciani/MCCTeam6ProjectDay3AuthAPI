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
	  
	    stage('Release - AuthService') {
	      sh 'gradle build -x test'
	      sh 'echo AuthService is ready to release!'
	    }
	  }
    }
}
