#!/usr/bin/env groovy

pipeline {
  agent any

  options {
    ansiColor('xterm')
    timestamps()
  }

  libraries {
    lib("pay-jenkins-library@master")
  }

  stages {

    stage('Docker Build') {
      when {
        branch 'master'
      }
      steps {
        script {
          buildAppWithMetrics{
            app = "publicapistub"
          }
        }
      }
      post {
        failure {
          postMetric("publicapistub.docker-build.failure", 1)
        }
      }
    }

    stage('Docker Tag') {
      when {
        branch 'master'
      }
      steps {
        script {
          dockerTagWithMetrics {
            app = "publicapistub"
          }
        }
      }
      post {
        failure {
          postMetric("publicapistub.docker-tag.failure", 1)
        }
      }
    }

  }
  post {
    failure {
      postMetric(appendBranchSuffix("publicapistub") + ".failure", 1)
    }
    success {
      postSuccessfulMetrics(appendBranchSuffix("publicapistub"))
    }
  }
}