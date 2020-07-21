#!/usr/bin/env groovy
node{
  //The first three stages occur no matter what branch a change occurs on
  def branch = "${env.BRANCH_NAME}"

  stage ('Checkout') {
    //checkout the project
    checkout scm
    echo "The working branch is: " + branch
  }

  stage ('Build') {
  //  nodejs('nodejs-14.2') {
      //build project dependencies
      sh 'npm -v'
      sh 'npm install'
   // }
  }

  stage ('Unit Tests') {
   // nodejs('nodejs-14.2') {
      browserstack('007ecb9e-8b9e-453d-9e2e-cb9d4e894383') {
        //run KarmaJS tests and then exit process with singleRun command
        sh 'npm run test-single-run'
      }
  //  }
  }

  stage('Execute E2E Tests on BS'){
    echo 'Place e2e test code here'
    // nodejs('nodejs-14.2') {
    //   browserstack('007ecb9e-8b9e-453d-9e2e-cb9d4e894383') {
    //     sh 'npm run wdio-bs'
    //   } 
    // }
  }
  

  if ("${env.BRANCH_NAME}".startsWith('release/') || "${env.BRANCH_NAME}".startsWith('hotfix')){
    stage('Product Owner Review') {
      input 'Waiting for Visual Testing analysis'
    }
  }
  stage('JUnit Reporter') {
    sh 'ls -lah app/karma_junit_reports/'
    junit 'app/karma_junit_reports/*.xml'
  }

  stage('Product Owner Review') {
      input 'Product Owner Review'
  }   
}
