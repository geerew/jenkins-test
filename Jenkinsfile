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
              spec:
                containers:
                - name: docker
                  image: docker:dind
                  command:
                  - cat
                  tty: true
                  securityContext:
                    privileged: true
                  volumeMounts:
                    - name: docker-sock
                      mountPath: /var/run/docker.sock
                volumes:
                  - name: docker-sock
                    hostPath:
                      path: /var/run/docker.sock
            '''
        } 
    }
  
    stages{
        // Build a docker image with the required tools. This runs on every branch
        stage("Build Docker Image") {
            steps {
                container('docker') {
                    script {
                         TAG = "${IMAGE_NAME}:${env.CHANGE_ID?'PR' + CHANGE_ID + '_':''}${BUILD_NUMBER}_${GIT_COMMIT.take(7)}"
                         IMG_REF = docker.build(TAG, '--pull -f Dockerfile .')
                    }
                }
            } 
        }
    }
}
