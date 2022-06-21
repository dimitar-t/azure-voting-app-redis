pipeline {
   agent any

   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Docker Build') {
         steps {
            sh(script: 'docker images -a')
            sh(script: """
               cd azure-vote/
               docker images -a
               docker build -t jenkins-pipeline .
               docker images -a
               cd ..
            """)
         }
      }
      stage('Start test app') {
         steps {
            sh(script: """
               docker-compose up -d
            """)
         }
         post {
            success {
               echo "App started successfully :)"
            }
            failure {
               echo "App failed to start :("
            }
         }
      }
      stage('Stop test app') {
         steps {
            sh(script: """
               docker-compose down
            """)
         }
      }
      stage("Push image to registry") {
         steps {
            echo("Workspace is $WORKSPACE")
            dir("$WORKSPACE/azure-vote") {
               docker.withRegistry("https://index.docker.io/v1", "DockerHub") {
                  def image = docker.build("dimitartachev23/jenkins-course")
                  image.push()
               }
            }
         }
      }
   }
}
