pipeline{
	agent any

	stages{
		stage('build'){
			steps{
				sh 'mvn clean package'
			}
			post{
				success{
					echo "Now Archiving ...."
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}
		stage('deploy-to-stage'){
			steps{
				build job: 'deply-to-stage'
			}
			
		}
		stage('Deploy to Production'){
			steps{
				timeout(time:5, unit:'DAYS'){
					input message: "Approve Production Deployment ?"
				}

				build job: 'Deploy-to-Prod'
			}
			post{
				success{
					echo "Code deployed to Production"
				}
				failure{
					echo "Code deployment failed"
				}
			}
		}
	}
}