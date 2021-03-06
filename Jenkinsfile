pipeline {
     agent any
     stages {
         stage('Build') {
             steps {
                 sh 'echo "Hello World"'
                 sh '''
                     echo "Multiline shell steps works too"
                     ls -lah
                 '''
             }
         }
         stage('Lint HTML') {
              steps {
                  sh 'tidy -q -e *.html'
              }
         }
         stage('Security Scan') {
              steps { 
                 sh 'whoami'
                 aquaMicroscanner imageName: 'alpine:latest', notCompliesCmd: 'exit 1', onDisallowed: 'fail', outputFormat: 'html'
              }
         } 
         stage('Upload to AWS') {
              steps {
                  withAWS(region:'ap-south-1') {
                  sh 'echo "Uploading content with AWS creds"'
                      s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'static-jenkins-pipeline-sagarnil')
                  }
              }
         }
         stage('Deliver for development to AWS') {
            when {
                branch 'Development'
            }
            steps {
                  withAWS(region:'ap-south-1') {
                  sh 'echo "Uploading content with AWS creds"'
                      s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index_development.html', bucket:'static-jenkins-pipeline-sagarnil')
                  }
              }
        }
        stage('Deploy for production to AWS') {
            when {
                branch 'Deployment'
            }
            steps {
                  withAWS(region:'ap-south-1') {
                  sh 'echo "Uploading content with AWS creds"'
                      s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index_deployment.html', bucket:'static-jenkins-pipeline-sagarnil')
                  }
              }
        }
     }
}
