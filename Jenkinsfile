node {
	stage('Code from git') {
    git 'https://github.com/gopichand9999/myweb'
    }
	stage('maven build'){
	def maven_home = tool name: 'maven-3.3.9', type: 'maven'
	def maven_bin = "${maven_home}/bin/mvn"
               
	sh "${maven_bin} clean package"
	
    }
	stage('BUILDING Docker image') {
	// sh "docker rm -f myweb" 
    // sh "docker rmi -f sureshbabu/myweb:0.0.1"
    sh "docker build -t sureshkanna/myweb:0.0.1 ."          
    }
    stage('PUSH TO DOCKER HUB') {
    //withCredentials([string(credentialsId: 'docker-pwd', variable: 'docker-pwd')]) {
        sh "docker login -u sureshkanna -p guptaig9d"
       // }
     sh "docker push sureshkanna/myweb:0.0.1 "
    }
    stage('RUN DOCKER CONTAIONER') {
    sh "docker run -d -p 90:80 --name myweb sureshkanna/myweb:0.0.1"          
    }
	 stage('Run Container on Dev Server'){
    // sh "sudo docker rm -f myweb" 
    // sh "sudo docker rmi -f sureshbabu/myweb:0.0.1"
     def dockerRun = 'docker run -d -p 90:80 --name myweb sureshkanna/myweb:0.0.1'
     sshagent(['ansiblepem']) {
     sh "ssh -o StrictHostKeyChecking=no ec2-user@18.188.112.86 sudo ${dockerRun}"
     }
   }
}
