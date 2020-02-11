pipeline {
  agent any

//  environment {
  //    PATH = "/opt/maven3/bin:$PATH"
  // }

  stages {
     stage("Git Checkout") {
	steps {
	   git credentialsId: 'github', url: 'https://github.com/nazir-alam/simple-maven-project.git'
	}
      }

      stage("Build") {
	steps {
	   sh "mvn clean package"
	   sh "mv target/*.war target/simple-maven-project.war"
	}
      }

      stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
	stage("deploy") {
	  steps {
	   sshagent(['tomcat']) {

	   sh """
		scp -o StrictHostKeyChecking=no target/simple-maven-project.war ec2-user@172.31.43.87:/opt/tomcat8/webapps/
		ssh username@ip_address /opt/tomcat8/bin/shutdown.sh
		ssh username@ip_address /opt/tomcat8/bin/start.sh

	      """
	   }
	  }
	}
      }
    }
