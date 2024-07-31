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

        stage('Push to quay'){
            steps {
                sh '''
                    oc start-build greeting-devsecops-quay --follow --wait -n ${APP_NAMESPACE}
                '''
            }
	    }    
    }

    post {
        failure {
            withCredentials([usernamePassword(
                credentialsId: 'github-global',
                usernameVariable: 'USERNAME',
                passwordVariable: 'PASSWORD'
            )]) {
                sh """
                curl -X POST \
                -H 'Authorization: token $PASSWORD' \
                'https://api.github.com/repos/$USERNAME/do400-greetingdevsecops/issues' \
                -d '{"title": "CI build $BUILD_NUMBER", "body": "Pipeline
                build $BUILD_NUMBER has failed"}'
                """
            }
        }
    }
}
