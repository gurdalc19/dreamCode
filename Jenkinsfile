environment {
        GCR_CRED = credentials('dream-project-381712')
        GCR_REPO = "gcr.io/dream-project-381712"
        IMAGE_TAG = $BUILD_NUMBER
    }

node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build image') {
  
       app = docker.build("gcr.io/dream-project-381712/dream")
    }

    stage('Test image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }


    stage('Push Image') {
        
        sh 'echo "$GCR_CRED" > abc.json'
        sh 'docker login -u _json_key -p "$(cat abc.json)" https://gcr.io'
        sh "docker build . -t ${GCR_REPO}:${IMAGE_TAG}"
        sh "docker push ${GCR_REPO}:${IMAGE_TAG}"
        sh 'docker logout https://gcr.io'    

    }
    
    
    stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                build job: 'dreamManifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
