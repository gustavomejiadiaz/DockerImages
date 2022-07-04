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
  powershell("""Set-Content -Path "C:\\ProgramData\\docker\\config\\daemon.json" -Value '{ "hosts": ["tcp://0.0.0.0:2375", "npipe://"],"max-concurrent-uploads": 10}'""")
  powershell("""Restart-Service docker""")

  // docker process pipeline
  Logger(this).trace """Searching for docker files: findFiles(glob: '**\\Source\\*\\Dockerfile', excludes: '**\\Package\\PackageTmp\\Dockerfile')"""
  def dockerfiles = findFiles(glob: '**\\Source\\*\\Dockerfile', excludes: "**\\Package\\PackageTmp\\Dockerfile")
  .collect{ 
    [dockerfile: it.path, path: it.path.split("\\\\")[0..-2].join("\\"), name: it.path.split("\\\\")[1..-2][0].replace(".", "-").toLowerCase() ]
  }

  def stepsForParallel = [:]
  def rmiCommandParameters = " ";
  dockerfiles.each { docker -> 
    rmiCommandParameters += " ${vars.dockerRegistry}/open/${docker.name}:${tag}"
    stepsForParallel[docker.name] = { ->
      dir(docker.path) {
        Logger(this).info "DOCKER: Building docker open/${docker.name}"
        //intappDockerBuild(appName: docker.name, appImage: "open/${docker.name}", tag: tag, skipDockerLogin: true)
      }
    }
  }

  parallel stepsForParallel

  dockerfiles.each { docker -> 
        Logger(this).info "DOCKER: Pushing docker open/${docker.name}"
        //intappDockerPush(appName: docker.name, appImage: "open/${docker.name}", version: tag, warnError: true, maxBuilds: -1, maxDays: 14)
        Logger(this).info "DOCKER: Completed docker open/${docker.name}"
  }

  Logger(this).info "Removing docker images from the agent to save space"
  powershell("docker rmi ${rmiCommandParameters} -f")
}
