pipeline {
    agent any 
     tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "localMaven"
        git "Default"
    }
    stages {
        stage('SCM Checkout') {
            steps{
                git branch: 'chatapp2',credentialsId:'git-chat', url: 'https://github.com/rayank07/chatbot2.git'
            }
        }
        stage('MVN Build') {
          steps{
             sh "mvn -Dmaven.test.failure.ignore=true clean package"
             sh "mv target/websocket-demo-0.0.1-SNAPSHOT.jar target/chatapp.jar"
          }
       }
       stage('MVN Testing') {
          steps{
             sh "mvn test"
          }
       }
       stage('test report') {
          steps{
             junit 'target/surefire-reports/*.xml'
          }
          post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
 }
stage('Get ansible code') {
          steps{
             
                git "https://github.com/gopal1409/ansible-tomcat-jenkins-script.git"
          }
       }
        stage('execute ansible') {
          steps{
             
                sshagent(['ssh-pass-ansible']) {
                 ansiblePlaybook inventory:  'dev.inv',disableHostKeyChecking: true,  playbook: 'tomcat.yml'
              }

          }
       }
}
}
