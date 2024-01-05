pipeline{
    agent any
    stages{
        stage("Maven Build"){
           steps{
               sh 'mvn package'
           } 
        }
        stage("Dev Deploy"){
           steps{
              sshagent(['tomcat']) {
                // Copy war file to tomcat dev server
                sh "scp -o StrictHostKeyChecking=no target/doctor-online.war ec2-user@172.31.18.234:/opt/tomcat9/webapps/"
                // Restart tomcat server
                sh "ssh ec2-user@ 172.31.18.234 /opt/tomcat9/bin/shutdown.sh"
                sh "ssh ec2-user@172.31.18.234 /opt/tomcat9/bin/startup.sh"
              }
           } 
        }
    }
}
