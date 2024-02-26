pipeline {
    agent {
        kubernetes {
            // Define your pod template
            yaml '''
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: stest
spec:
  containers:
  - name: jenkins-agent
    image: jenkins/inbound-agent:latest
    command:
    - cat
    tty: true
  - name: docker
    image: docker:dind
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

    environment {
        IMAGE_NAME = 'mike'
        // Default TAG if other values are not available
        TAG = 'latest'
        // Docker image reference
        IMG_REF = ''
        // Path to Dockerfile
        DOCKERFILE = 'Dockerfile'
    }

    stages {
        // Build a docker image with the required tools. This runs on every branch
        stage('Build Docker Image') {
            steps {
                // Switch to the Docker container to perform Docker operations
                container('docker') {
                    script {
                        // Construct the image tag
                        TAG = "${IMAGE_NAME}:${env.CHANGE_ID ? 'PR' + env.CHANGE_ID + '_' : ''}${env.BUILD_NUMBER}_${env.GIT_COMMIT.take(7)}"
                        // Build the Docker image
                        IMG_REF = docker.build(TAG, "--pull -f ${DOCKERFILE} .")
                    }
                }
            }
        }
    }
}
