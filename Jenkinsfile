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
    //build project dependencies
    sh 'npm -v'
    sh 'npm install'
  }

  stage ('Unit Tests') {
     echo 'Place code for Unit Tests here'
  }

  stage('Execute E2E Tests on BS'){
    echo 'Place e2e test code here' 
    browserstack(credentialsId: 'browserstack_access_key',
                 localConfig: [localOptions: '--force-local',
                 //localPath: '/path/to/local'
                              ])
    {
    // code for executing test cases
    }
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
