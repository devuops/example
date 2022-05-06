pipeline {
    options {
        disableConcurrentBuilds()
    }
    agent {
        kubernetes {
            label 'docker-in-docker-maven'
            yaml """
apiVersion: v1
kind: Pod
spec:
containers:
- name: git
  image: alpine/git
  command: [cat]
- name: kaniko
  image: docker:19.03.1-dind
  env:
    - name: DOCKER_TLS_CERTDIR
      value: ""
  securityContext:
    privileged: true
  volumeMounts:
      - name: cache
        mountPath: /var/lib/docker
volumes:
  - name: cache
    hostPath:
      path: /tmp
      type: Directory
"""
        }
    }
    stages {

        stage('Docker Build') {
            steps {
                container('docker-client') {
                    sh ' echo "from alpine:laster" >> dockerfile
                    sh 'docker version && DOCKER_BUILDKIT=1 docker build --progress plain -t testing .'
                }
            }
        }
    }
}
