pipeline {
environment {
    JFROG_URL = 'https://trialcxid14.jfrog.io/artifactory'
    REPO_NAME = 'parcel-libs-snapshot'      // JFrog repo for feature branches
	 
  }
    agent { label 'slave1' }
    stages {
        stage('Checkout') {
            steps {
                sh "rm -rf Parcel-service"
				 sh "git clone https://github.com/ManasaMarigowda/Parcel-service"
            }
        } 
 

       stage('Create Versioned Artifact') {
      steps {
        script {
          def sha = sh(
            script: 'git rev-parse --short HEAD',
            returnStdout: true
          ).trim()

          def branchSafe = env.BRANCH_NAME.replaceAll('[^a-zA-Z0-9_.-]', '_')

          env.ARTIFACT = "Parcel-service-${env.BUILD_NUMBER}-${sha}.jar"
          sh "cp /home/ubuntu/Parcel-service/target/*.jar ${env.ARTIFACT}-f1.jar"
		  sh "cp /home/ubuntu/Parcel-service/target/*.jar ${env.ARTIFACT}-f2.jar"
			
         
          archiveArtifacts artifacts: "${env.ARTIFACT}-f1", fingerprint: true
			archiveArtifacts artifacts: "${env.ARTIFACT}-f2", fingerprint: true
        }
      }
    }

    stage('Upload to JFrog') {
      steps {
        withCredentials([string(credentialsId: 'jfrogkey', variable: 'JFROG_API_KEY')]) {
          sh """
            curl -f -H "X-JFrog-Art-Api: ${JFROG_API_KEY}" \
                -T "${env.ARTIFACT}-f1.jar" \
                "${JFROG_URL}/${REPO_NAME}/${env.BRANCH_NAME}/${env.ARTIFACT}-f1.jar"
          """
			 sh """
            curl -f -H "X-JFrog-Art-Api: ${JFROG_API_KEY}" \
                -T "${env.ARTIFACT}-f2.jar" \
                "${JFROG_URL}/${REPO_NAME}/${env.BRANCH_NAME}/${env.ARTIFACT}-f2.jar"
          """
        }
      }
    }
       

   

}

}
