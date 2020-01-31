def groovy

pipeline {
  agent none
  stages {

    stage('Load groovy script') {
      agent any
      stages {
        stage('Checkout') {
          steps {
            checkout scm
            script {
              groovy = load('releng/Jenkinsfile.groovy')
            }
          }
        }
      }
    }

    stage('Launch agent') {
      agent {
        kubernetes {
          label 'rcptt-build-and-deploy-agent-3.5.4'
          yaml "${env.YAML_BUILD_AND_DEPLOY_AGENT}"
        }
      }
      stages {
        stage('Start Test') {
          steps {
            script {
              groovy.tests("https://github.com/xored/org.eclipse.rcptt.rap.tests", RUNNER, "-DwarPath=${WAR_PATH}")
            }
          }
        }
      }
      post {
        always {
          script {
            groovy.post_build_actions()
          }
        }
      }
    }

  }
}