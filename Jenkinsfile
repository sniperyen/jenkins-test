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
metadata:
  labels:
    some-label: some-label-value
spec:
  containers:
  - name: maven
    image: maven:alpine
    command:
    - cat
    tty: true
  - name: busybox
    image: busybox
    command:
    - cat
    tty: true
"""
    }
  }
  stages {
    stage('Build') {
      steps {
        echo 'make package'
        container('maven') {
          sh 'mvn -version'
        }
        container('busybox') {
          sh '/bin/busybox'
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
