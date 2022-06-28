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

def docker(tag) {
  Logger(this).log "Docker version: $tag"

  // enable docker engine on port
  Logger(this).log "Configuring docker engine for tcp://0.0.0.0:2375"
  
}
