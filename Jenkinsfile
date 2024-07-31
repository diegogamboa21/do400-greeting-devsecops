pipeline {
    agent { label 'nodejs' }

    environment { APP_NAMESPACE = 'ihvdgq-devsecops' }

    stages{

        stage('Test'){
            steps {
                sh "node test.js"
            }
        }

        stage('deploy'){
	   steps {
		sh '''
		    oc start-build greeting-devsecops --follow --wait -n ${APP_NAMESPACE}
		'''
	   }
	}
    }
}
