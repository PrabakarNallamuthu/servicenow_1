pipeline {
  agent any
  environment {
    APPSYSID = '27d0e777c3805650a28cfeac05013197'
    BRANCH = "${BRANCH_NAME}"
    CREDENTIALSDEV = 'servicenow'
    CREDENTIALSTEST = 'ServiceNow'
    DEVENV = 'https://dev263447.service-now.com/'
    TESTENV = 'https://dev196634.service-now.com/'
    PRODENV = 'https://dev196634.service-now.com/'
    // TESTSUITEID = 'b1ae55eedb541410874fccd8139619fb'
  }
  stages {
    stage('Build') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snApplyChanges(appSysId: "${APPSYSID}", branchName: "${BRANCH}", url: "${DEVENV}", credentialsId: "${CREDENTIALSDEV}")
        snPublishApp(credentialsId: "${CREDENTIALSDEV}", appSysId: "${APPSYSID}", obtainVersionAutomatically: true, url: "${DEVENV}")
      }
    }
    stage('Test') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALSTEST}", url: "${TESTENV}", appSysId: "${APPSYSID}")
        // snRunTestSuite(credentialsId: "${CREDENTIALSTEST}", url: "${TESTENV}", testSuiteSysId: "${TESTSUITEID}", withResults: true)
      }
    }
    stage('Deploy to Prod') {
      when {
        branch 'master'
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALSTEST}", url: "${PRODENV}", appSysId: "${APPSYSID}")
      }
    }
  }
}
