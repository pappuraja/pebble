node{
   stage('scm checkout'){
   git credentialsId: 'github-auth', url: 'https://github.com/pappuraja/pebble.git'
   }

    stage('mvn package'){
    def mvnHome = tool name: 'maven2', type: 'maven'
	def mvnCMD = "${mvnHome}/bin/mvn"
    sh "${mvnCMD} clean package -Dmaven.test.skip=true"
     
   }
stage('Build Docker Image'){
     sh 'docker build -t prsdocker143/pebble:v2 .' 
   }
   
   stage('Push Docker Image'){
     withCredentials([string(credentialsId: 'docker-hub-password', variable: 'dockerhub')]) {
     sh "docker login -u prsdocker143 -p ${dockerhub}"
   }
     sh 'docker push prsdocker143/pebble:v2'
   }
 stage('Deploy container on target server'){
/*   sshagent(['ec2-credentials']){
	 sh 'ssh -o StrictHostKeyChecking=no ec2-user@<privateip>'
	} */
     sh 'docker run -it -d  --name pebble -p  8081:8080 prsdocker143/pebble:v2'
     sh 'docker run -it -d  --name pebble1 -p 8082:8080 prsdocker143/pebble:v2'
     sh 'docker run -it -d  --name pebble2 -p 8083:8080 prsdocker143/pebble:v2'
	 
   }
}
