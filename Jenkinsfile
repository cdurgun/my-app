
node {
   // This is to demo github action	
   def sonarUrl = 'sonar.host.url=http://165.22.203.249:9000'
   def mvn = tool (name: 'jenkins-maven', type: 'maven') + '/bin/mvn'
   stage('SCM Checkout'){
    // Clone repo
	git branch: 'master', 
	credentialsId: 'github', 
	url: 'https://github.com/cdurgun/my-app'
   
   }
   
   stage('SonarQube Analysis') {
        def mvnHome =  tool name: 'jenkins-maven', type: 'maven'
        withSonarQubeEnv('sonarqube') { 
          sh "${mvnHome}/bin/mvn sonar:sonar"
        }
    }
   
   stage("Quality Gate Status Check"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
	        slackSend baseUrl: 'https://hooks.slack.com/services/',
       		channel: '#jenkins-pipeline-demo',
       		color: 'good', 
       		message: 'SonarQube Analysis Failed !', 
       		teamDomain: 'cdurgun',
       		tokenCredentialId: 'slack-demo'    
                error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }        	

   stage('Email Notification'){
		mail bcc: '', body: """Hi Team, You build successfully deployed
		                       Job URL : ${env.JOB_URL}
							   Job Name: ${env.JOB_NAME}

Thanks,
DevOps Team""", cc: '', from: '', replyTo: '', subject: "${env.JOB_NAME} Success", to: 'cdurgun@gmail.com'
   
   }
}

