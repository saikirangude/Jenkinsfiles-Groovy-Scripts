
node {
   // This is to demo github action	
#   def sonarUrl = 'sonar.host.url=http://54.237.204.218:9000'
   def mvn = tool (name: 'MVN-3.6.1', type: 'maven') + '/bin/mvn'
   stage('SCM Checkout'){
    // Clone repo
	git branch: 'master', 
#	credentialsId: 'github', 
	url: 'https://github.com/saikirangude/Git-repo2.git'
   
   }
   
#   stage('Sonar Publish'){
#	   withCredentials([string(credentialsId: 'sonarqube', variable: 'sonarToken')]) {
#        def sonarToken = "sonar.login=${sonarToken}"
#        sh "${mvn} sonar:sonar -D${sonarUrl}  -D${sonarToken}"
#	 }
#     
#   }
   
	
   stage('Mvn Package'){
	   // Build using maven
	   
	   sh "${mvn} clean package deploy"
   }
   
   stage('deploy-dev'){
       def tomcatDevIp = '54.237.204.218:8080'
	   def tomcatHome = '/tomcat8/'
	   def webApps = tomcatHome+'webapps/'
	   def tomcatStart = "${tomcatHome}bin/startup.sh"
	   def tomcatStop = "${tomcatHome}bin/shutdown.sh"
	   
	   sshagent (credentials: ['Tomcat-Deploy-Credentials']) {
	      sh "scp -o StrictHostKeyChecking=no target/myweb*.war ec2-user@${tomcatDevIp}:${webApps}trucks.war"
          sh "ssh ec2-user@${tomcatDevIp} ${tomcatStop}"
		  sh "ssh ec2-user@${tomcatDevIp} ${tomcatStart}"
       }
   }
   stage('Email Notification'){
		mail bcc: '', body: """Hi Team, You build successfully deployed
		                       Job URL : ${env.JOB_URL}
							   Job Name: ${env.JOB_NAME}

Thanks,
DevOps Team""", cc: '', from: '', replyTo: '', subject: "${env.JOB_NAME} Success", to: 'saikirangude@gmail.com'
   
   }
}

