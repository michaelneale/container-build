node {
    
    stage "Build"
       sh "echo here"
       
    stage "Test"
        parallel (
            "Firefox" : {
                sh "echo ffox"
            },
            "Chrome" : {
                sh "echo chrome"
            }
        )
        
    stage "Deploy"
        sh "echo deploying"
    
}

