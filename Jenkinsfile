String IMAGE_NAME = "mike"
String TAG
def IMG_REF

String STRIPPED_URL

pipeline {
    agent { 
        kubernetes {
            yaml '''
              apiVersion: v1
              kind: Pod
              metadata:
                labels:
                  some-label: some-label-value
              spec:
                containers:
                - name: docker
                  image: docker:latest
                  command:
                  - cat
                  tty: true
            '''
        } 
    }
  
    stages{
        // Build a docker image with the required tools. This runs on every branch
        stage("Build Docker Image") {
            steps {
                container('docker') {
                    sh 'env'
                    sh 'which docker'
                    sh 'pwd'
                    sh 'ls -l'
                    script {
                         TAG = "${IMAGE_NAME}:${env.CHANGE_ID?'PR' + CHANGE_ID + '_':''}${BUILD_NUMBER}_${GIT_COMMIT.take(7)}"
                         IMG_REF = docker.build(TAG, '--pull -f Dockerfile .')
                    }
                }
            } 
        }
    }
}
