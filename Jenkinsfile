stage "Build"
    node {
       sh "echo here" 
    }
       

stage "Test"
parallel (
    "Firefox" : {
        node {
            sh "echo ffox"    
        }
        
    },
    "Chrome" : {
        node {
            sh "echo chrome"    
        }
        
    }
)
        
        
        
        
stage "Deploy"
node {
    sh "echo deploying"
}
        
    

