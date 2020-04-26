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
      parallel {
        stage('Lint HTML') {
          steps {
            sh 'tidy -q -e *.html'
          }
        }

        stage('') {
          steps {
            sh '''<!doctype html>
    <html>
      <head>
        <title>Static HTML Site</title>
      </head>
      <body>
        <p>This is the first paragraph in a simple Static HTML site. There is no <strong>CSS</strong> or <strong>JavaScript</script>.</p>
      </body>
    </html>'''
          }
        }

      }
    }

    stage('Security Scan') {
      steps {
        aquaMicroscanner(imageName: 'alpine:latest', notCompleted: 'exit 1', onDisallowed: 'fail')
      }
    }

    stage('Upload to AWS') {
      steps {
        withAWS(region: 'us-east-2', credentials: 'aws-static') {
          sh 'echo "Uploading content with AWS creds"'
          s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file: 'index.html', bucket: 'static-jenkins-pipeline')
        }

      }
    }

  }
}