pipeline {
  options {
    skipDefaultCheckout()
  }
  agent {
    kubernetes {
      cloud 'kubernetes-cluster'
      label 'kubeprod'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
spec:
  securityContext:
    runAsUser: 1000
    fsGroup: 1000
  containers:
  - name: go
    image: golang:1.10.1-stretch
    stdin: true
    command: ['cat']
    resources:
      limits:
        cpu: 2000m
        memory: 2Gi
      requests:
        # rely on burst CPU
        cpu: 10m
        # but actually need ram to avoid oom killer
        memory: 1Gi
  - name: az
    image: microsoft/azure-cli:2.0.45
    stdin: true
    command: ['cat']
    resources:
      limits:
        cpu: 100m
        memory: 500Mi
      requests:
        cpu: 1m
        memory: 100Mi
"""
    }
  }
  stages {
    stage('Build') {
      steps {
        echo 'make package'
        container('go') {
          sh 'go version'
        }
      }
    }
    stage('Test') {
      steps {
        echo 'make check'
      }
    }
    stage('Deploy') {
      when { tag "*" }
      steps {
        echo 'make deploy'
      }
    }
  }
}
