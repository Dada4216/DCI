node{
 stage('Git Checkout'){
	//git 'https://github.com/javahometech/my-app'  
	  git credentialsId: '135408ed-1e65-4880-a6f7-d247f1b6e2b5', url: 'http://10.112.78.16/wabled/Docker-Compose-Integration.git'

 }
 
 stage('Build Docker Imager'){
   //sh 'docker build -t kammana/myweb:0.0.1 .'
     sh '/usr/local/bin/docker-compose build'

 }
 
 stage('Create and start the containers locally if required'){
         //sh '/usr/local/bin/docker-compose -f docker-compose.yml up'

 }
 
 stage('Stopping the Containers'){
         sh 'docker-compose down'

 }
  stage('Showing all Generated Images'){
         sh 'docker-compose images'

 }
 
  stage('Showing all generated containers'){
         sh 'docker-compose ps'

 }
 stage('Push to Docker Registry'){
 
	 withCredentials([string(credentialsId: 'github-pwd', variable: 'dockerHubPwd')]) {
        //sh "docker login -u kammana -p ${dockerHubPwd}"
        sh 'docker login -u admin -p admin123'
   
     }
	 sh 'docker push kammana/myweb:0.0.1'
 }
 stage('Remove Previous Container'){
	try{
		def dockerRm = 'docker rm -f myweb'
		sshagent(['docker-dev']) {
			sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.17.196 ${dockerRm}"
		}
	}catch(error){
		//  do nothing if there is an exception
	}
 }
 stage('Deploy to Dev Environment'){
   def dockerRun = 'docker run -d -p 8080:8080 --name myweb kammana/myweb:0.0.1 '
   sshagent(['docker-dev']) {
    sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.17.196 ${dockerRun}"
   }

 }

}


