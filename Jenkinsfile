

inContainer
{
        image = "cloudbees/java-build-tools" //optional, or will loolk for Dockerfile
        script = 'echo hey > $PWD/data'
        archive = '$PWD/data'
        testScript = "echo pass"
        // X label - do outside in node
        // X testResults         
        // X git THINK: need to do checkout scm, but sometimes supress it... hrm can just do git checkout outside if needed... or check for .git.. 
        // retry
        // timeout
        // dockerfile check for file existence? 
        // branch
        // hipchat
        // slack
        // email
        // deployScript
        
}    

/** DSL with auto stages */
def inContainer(body) {  
  inContainer_impl(body, true)
}

/** DSL without stages */
def container(body) {
  inContainer_impl(body, false)
}




def inContainer_impl(body, useStages) {
    def config = [:]
    body.resolveStrategy = Closure.DELEGATE_FIRST
    body.delegate = config
    body()
    
    /** exit early, and exit often */
    if (config.branch && config.branch != env.BRANCH_NAME) return
    if (currentBuild.result == 'UNSTABLE') return
    
    if (useStages) stage "Preparing container"        
    if (config.image) {
      def image = docker.image(config.image)
    } else {
      def image = docker.build("build")
    }
    def image = docker.image(config.image)
    
    image.inside {
        if (useStages) {
          stage "Building"
          checkout scm
        }  
          
        try {
          sh config.script
        } catch (e) {
          nofityFailure(e, config)
          throw e
        }   
        
        try {
          if (config.testScript) {
            if (useStages) stage "Testing"
            sh config.testScript
          } 
        } catch (e) {
          currentBuild.result = 'UNSTABLE'                    
        }

        maybeCollectTestReport(config)            
        
        /** we don't want to continue if unstable */
        if (currentBuild.result == 'UNSTABLE') {
          notifyUnstable(config)
          return 
        }

        if (config.archive) {
            if (useStages) stage "Archiving"
            archive config.archive
        }          

        notifyFinished(config)
        
        
    }

}

def maybeCollectTestReport(config) {
  if (config.testResults) {
    step([$class: 'JUnitResultArchiver', testResults: config.testResults])
  } 
}

def notifyFinished(config) {
  echo "result was ${currentBuild.result}"
}

def notifyFailure(e, config) {
  echo "failed ZOMFG"
}

def notifyUnstable(config) {
  echo "unstable ZOMG"
}


/**

env.FOO = 42

node {
    container {
            image = "cloudbees/java-build-tools"
            script = 'echo hey > $PWD/data'
            archive = '$PWD/data'
            testScript = 'echo $FOO'
    }    
    stash name: "binary", includes: "data"
}

node {
    unstash 'binary'
    container {
        image = "cloudbees/java-build-tools"
        script = "ls -lah"
    }
    
}

*/
        
    
