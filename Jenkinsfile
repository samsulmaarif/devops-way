pipeline {
    agent any
    environment {
		STAGINGSERVER = '178.128.223.0'
	}
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build'
                archiveArtifacts artifacts: 'src/index.html'
            }
        }
        stage('DeployToStage') {
            when {
                branch 'master'
            }
            steps {
                sh 'scp src/* deployer@$STAGINGSERVER:/home/deployer/staging/html/'
            }

        }
        stage('DeployToProd') {
            when {
                branch 'master'
            }
            steps {
                input 'Does the staging environment look OK?'
                milestone(1)
                sh 'scp src/* deployer@$STAGINGSERVER:/home/deployer/production/html/'
            }
        }
    }
}
