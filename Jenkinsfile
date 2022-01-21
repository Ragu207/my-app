node{
   stage('SCM Checkout'){
     git 'https://github.com/Ragu207/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   stage('Build Docker Imager'){
   sh 'docker build -t ragu07/ram .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u ragu07 -p ${dockerPassword}"
    }
   sh 'docker push ragu07/ram'
   }
   stage('Nexus Image Push'){
   withCredentials([string(credentialsId: 'nexuspass', variable: 'nexuspassword' 52.66.130.51:8084)]) {
   sh "docker login -u admin -p ${dockerPassword} 52.66.130.51:8084" }
   sh "docker tag ragu07/ram 52.66.130.51:8084/demo"
   sh 'docker push 52.66.130.51:8084/demo'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest ragu07/ram' 
   }
}
}
