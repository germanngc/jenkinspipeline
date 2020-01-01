pipeline {
    agent any

	parameters {
		string (name: 'tomcat_dev', defaultValue: '18.191.113.19', description: 'Tomcat Stage Server')
		string (name: 'tomcat_prod', defaultValue: '3.135.223.118', description: 'Tomcat Prod Server')
	}

	triggers {
		pollSCM ('* * * * *')
	}

	stages {
		stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
			
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

		stage ('Deployments') {
			parallel {
				stage ('Deploy to Staging') {
					steps {
						sh "scp -i /root/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
					}
				}

				stage ('Deploy to Production') {
					steps {
						sh "scp -i /root/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
					}
				}
			}
		}
	}
}
