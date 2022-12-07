pipeline {
    agent any

    stages {
        stage('checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/uday898989/jenkins-project-git.git']]])
            }
        }        
        stage('Clean') {
            steps {
               sh "mvn -Dmaven.test.failure.ignore=true clean"
            }
        }
        
        stage('Validate') {
            steps {
                sh "mvn validate"
            }
        }
        
        stage('Compile') {
            steps {
                sh ('mvn compile');
            }
        }
        
        stage('Test') {
            steps {
                sh ('mvn test');
            }
        }
        
        stage('Package') {
            steps {
                sh ('mvn package');
            }
        }
      
        stage('Verify') {
            steps {
                sh ('mvn verify');
            }
        }
         stage('Install') {
            steps {
                sh ('mvn install');
            }
        }
            
        stage('deploy') {
            steps {
                sshagent(['deploy_user']) {
                   sh "scp -o StrictHostKeyChecking=no -T target/**.war target/01-maven-web-app.war ubuntu@34.227.86.216:/opt/tomcat/webapps"
                    
                       }
            }
        }
        
       stage('Stage-9 : Deployment - Deploy a Artifact devops-3.0.0-SNAPSHOT.war file to Tomcat Server') { 
            steps {
                sh 'curl -u admin:redhat@123 -T target/**.war "http://34.227.86.216:8080/manager/text/deploy?path=/uday&update=true"'
            }
        } 
  
          stage('Stage-10 : SmokeTest') { 
            steps {
                sh 'curl --retry-delay 10 --retry 5 "http://34.227.86.216:8080/uday"'
            }
        }   
    }
}
