pipeline{
    agent any
    
    stages{
        stage("Git Checkout"){
            steps{
                git credentialsId: 'github', url: 'https://github.com/kishanth94/javawebapplication'
            }
        }
	    
	/*   
        stage('Quality Gate Status Check'){
            steps{
                script{
			            withSonarQubeEnv(credentialsId: 'sonar-token') { 
                        // Get Home Path of Maven 
                        def mvnHome = tool name: 'maven-3', type: 'maven'
			            sh "${mvnHome}/bin/mvn clean sonar:sonar"
                       	  }
			            timeout(time: 1, unit: 'HOURS') {
			            def qg = waitForQualityGate()
				                if (qg.status != 'OK') {
					                 error "Pipeline aborted due to quality gate failure: ${qg.status}"
				                }
                          }
                }
            }  
        }
	*/
  
        stage("Maven Build"){
            steps{
                script{
                // Get Home Path of Maven 
                def mvnHome = tool name: 'maven-3', type: 'maven'
                sh "${mvnHome}/bin/mvn clean package"
                //sh "mv target/*.war target/myweb.war"
                }
            }
        }
	    
	stage("Upload War To Nexus"){
	    steps{
		script{
		    nexusArtifactUploader artifacts: [
			[
			    artifactId: 'javawebapplication', 
			    classifier: '', 
			    file: 'target/javawebapplication-1.0.0.war', 
			    type: 'war'
			]
		    ], 
	            credentialsId: 'nexus3', 
	            groupId: 'in.javahome', 
	            nexusUrl: '172.31.92.148:8081', 
	            nexusVersion: 'nexus3', 
	            protocol: 'http', 
		    repository: 'javawebapplication-release', 
		    version: '1.0.0'
		    }
	    }
	}	
        /*
        stage("deploy"){
            steps{
                sshagent(['aws-ec2-keypair']) {
                sh """
                    scp -o StrictHostKeyChecking=no target/myweb.war  ec2-user@172.31.80.47:/opt/tomcat8/webapps/
                    
                    ssh ec2-user@172.31.80.47 /opt/tomcat8/bin/shutdown.sh
                    
                    ssh ec2-user@172.31.80.47 /opt/tomcat8/bin/startup.sh
                
                """
                }
            
            }
        }
	*/
    }
}
