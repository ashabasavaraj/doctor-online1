pipeline{
    agent any
    stages{
        stage("Maven Build"){
           steps{
               sh 'mvn package'
           } 
        }
        
 stage("Upload Artifacts"){
            steps{
               script{
                    def pom = readMavenPom file: 'pom.xml'
                    def version = pom.version
                    def repoName = version.endsWith("SNAPSHOT") ? "do-snapshot": "do-release"
                    nexusArtifactUploader artifacts: [[artifactId: 'doctor-online', classifier: '', file: 'target/doctor-online.war', type: 'war']], 
                        credentialsId: 'nexus3', 
                        groupId: 'in.javahome', 
                        nexusUrl: '172.31.39.0:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: repoName, 
                        version: version
               }
            }
        }
        
        stage("Dev Deploy"){
           steps{
              sshagent(['tomcat']) {
                // Copy war file to tomcat dev server
                sh "scp -o StrictHostKeyChecking=no target/doctor-online.war ec2-user@172.31.18.234:/opt/tomcat9/webapps/"
                // Restart tomcat server
                sh "ssh ec2-user@172.31.18.234 /opt/tomcat9/bin/shutdown.sh"
                sh "ssh ec2-user@172.31.18.234 /opt/tomcat9/bin/startup.sh"
              }
           } 
        }
    }
}
