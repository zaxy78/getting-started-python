node ('python') {

    withCredentials([[$class: 'StringBinding', credentialsId: 'f5e83850-62a3-430c-b5b8-3351e9894df4', variable: 'SECRET']]) {

        def setup = {
            git url: 'https://github.com/googlecloudplatform/getting-started-python'
            env.PATH="${env.PATH}:/root/google-cloud-sdk/bin"
            env.GOOGLE_APPLICATION_CREDENTIALS="/etc/gcloud-service-account/gcloud-service-account"
            sh "./decrypt-secrets.sh ${env.SECRET}"
    
            sh "gcloud auth activate-service-account --key-file=/etc/gcloud-service-account/gcloud-service-account"
            sh "gcloud config set project verdant-future-95122"  
            sh "pip install nox-automation tox"     
        }

      
        branches = [:]
        
        branches[0] = { 
            node ('python') {
                setup()          
                sh "cp config.py 1-hello-world/; cd 1-hello-world; tox"
            }    
        }
        
        branches[1] = {
            node('python') {
                setup()          
                sh "cp config.py 2-structured-data/; cd 2-structured-data; tox"
            }
        }
        parallel(branches)
    }
    
}

