pipeline {
    agent any
    tools {
        maven "Maven"
    }
    stages {
        stage ("Initialize") {
            steps{
                sh '''
                    
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${PATH}"

                '''
            }
        }
        stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage ('Deploy-to-Tomcat') {
            steps{
                    // // new update check
                    // sh '''
                    //     sudo chmod -R 755 /root/prod/apache-tomcat-9.0.76/webapps
                    //     sudo chown -R ubuntu:ubuntu /root/prod/apache-tomcat-9.0.76/webapps
                    // '''
                    // sudo setfacl -m g:ubuntu:rwx -R /root/
                    
                    
                    sh sshagent(credentials: ['27b86657-ba75-4b78-9ad6-8a9146bfbb3a']) {
                    sh 'ssh root@tomcat-server "sudo systemctl stop tomcat"'
                    sh 'ssh root@tomcat-server "rm -rf /opt/tomcat/webapps"'
                    sh 'mkdir -p deploy'
                    sh 'scp /var/lib/jenkins/workspace/deploy root@13.127.232.105:/opt/tomcat/webapps'
                    sh 'ssh root@13.127.232.105 "sudo systemctl start tomcat"'
                    ssh -oStrictHostKeyChecking=no host
                }
           }
             stage('Verify') {
             steps {
             Perform additional verification or integration tests
            sh 'curl http://13.127.232.105:8080/'
      }
    }


            
