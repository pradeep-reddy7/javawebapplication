pipeline{
    agent any
    
    stages{
        stage("Git Checkout"){
            steps{
                git credentialsId: 'github', url: 'https://github.com/kishanth94/javawebapplication'
            }
        }
        stage("Maven Build"){
            steps{
                def mvnHome = tool name: 'maven-3', type: 'maven'
                sh "${mvnHome}/bin/mvn clean package"
                sh "mv target/*.war target/myweb.war"
            }
        }
        stage("deploy"){
            steps{
                sshagent(['tomcat-new']) {
                sh """
                    scp -o StrictHostKeyChecking=no target/myweb.war  ec2-user@172.31.30.253:/opt/tomcat8/webapps/
                    
                    ssh ec2-user@172.31.30.253 /opt/tomcat8/bin/shutdown.sh
                    
                    ssh ec2-user@172.31.30.253 /opt/tomcat8/bin/startup.sh
                
                """
                }
            
            }
        }
    }
}
