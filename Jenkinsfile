String IMAGE_NAME = "mike"
String TAG
def IMG_REF

String STRIPPED_URL

pipeline {
    agent { label 'docker' }
  
    stages{
        // Build a docker image with the required tools. This runs on every branch
        stage("Build Docker Image") {
            steps {
                sh 'which docker'
                sh 'hostname'
                sh 'ps -ef'
                
                script {
                    TAG = "${IMAGE_NAME}:${env.CHANGE_ID?'PR' + CHANGE_ID + '_':''}${BUILD_NUMBER}_${GIT_COMMIT.take(7)}"
                    IMG_REF = docker.build(TAG, '--pull -f Dockerfile .')
                }
            }
        }
    }
}
