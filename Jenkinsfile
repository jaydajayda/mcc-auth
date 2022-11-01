node {
    stage ("Git Checkout"){
        git branch: 'main', url: 'https://github.com/jaydajayda/mcc-auth.git'
    }
    
    stage ("Gradle Build") {
        sh 'gradle clean build'
    }
    
    stage ("Gradle bootJar-Package") {
        sh 'gradle bootjar'
    }
    
    stage ("Containerize the app-docker build") {
        sh 'docker build --rm -t mccauth:v1.0 .'
    }
    
    stage ("Inspect the docker image"){
        sh "docker images mccauth:v1.0"
        sh "docker inspect mccauth:v1.0"
    }
    
    stage ("Run Docker container instance - AuthApi"){
        sh "docker run -d --rm --name mccauth -p 8081:8081 mccauth:v1.0"
     }
    
    stage('User Acceptance Test') {
	
	  def response = input message: 'Is this build good to go?',
	   parameters: [choice(choices: 'Yes\nNo', 
	   description: '', name: 'Pass')]
	
	  if(response=="Yes") {
	    stage('Deploy to Kubernetes cluster') {
	      sh "kubectl create deployment mccauth --image=mccauth:v1.0"
	      sh "kubectl set env deployment/mccauth DATA_API_HOST=\$(kubectl get service/mccdata -o jsonpath='{.spec.clusterIP}'):8080"
	      sh "kubectl expose deployment mccauth --type=LoadBalancer --port=8081"
	    }
	  }
    }

    stage("Production Deployment View"){
        sh "kubectl get all"
    }
  
}