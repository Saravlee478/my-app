node{
   stage('SCM Checkout'){
     git 'https://github.com/Saravlee478/my-app'
   }
    stage('war file a kudu'){
      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }}
stage('Build Docker Image'){
   sh 'docker build -t saravlee/dc10192022:0.1 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u saravlee -p ${dockerPassword}"
    }
   sh 'docker push saravlee/dc10192022:0.1'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 44.210.137.186:8083"
   sh "docker tag saravlee/dc10192022:0.1 44.210.137.186:8083/saravlee:1.0"
   sh 'docker push 44.210.137.186:8083/saravlee:1.0'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f container1'
	}
	catch(error)
	{		//  do nothing if there is an exception
	}
   }
   stage('Docker deploy pannu'){
   sh 'docker run -d -p 8090:8080 --name container1 saravlee/dc10192022:0.1' 
   }
}
