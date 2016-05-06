stage "Build"
    if (env.BRANCH_NAME != "master") return
    node {
       sh "echo here2" 
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
        
    

