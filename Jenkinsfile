#!groovy

import hudson.tasks.test.AbstractTestResultAction
import groovy.json.JsonSlurper


// regex patterns for full match
def buildExcludePaths = [
  "Documentation/.*",
  "TestPlans/.*",
  ".*\\.docx",
  ".*\\.css"
]

def unittestExcludePaths = [
  "Deploy/.*",
  ".*\\.feature",
  "Source/Wilco\\.UITest/.*",
  "Source/DevTestAutomation/.*",
  ".*\\.docx",
  ".*\\.css",
  ".*\\.js"
]

def uitestExcludePaths = [
  "Deploy/.*",
  ".*\\.feature"
]

vars {
  vars.product = "Open"
  vars.appImage = "open/open"
  vars.buildNodeName = "openBuildv4"
  vars.workspace = "gitlab/opnv3"
  vars.slackChannel = '#open-jenkins'

  vars.build = currentBuild.number

  // DOK-10961 | hack for release branches. The same Artifactory retention rules apply for releases and feature branches.
  vars.releaseBranchPattern = /([$]+)/

  // NBI-35160 | Long running unit-tests we should launch only in master branch.
  vars.isMasterBranch = env.BRANCH_NAME == "master"
  vars.skipBuild = false
  vars.skipUnitTests = false
  vars.skipUITests = false

}
