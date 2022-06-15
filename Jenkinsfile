pipeline {
   agent any

   stages {
      stage('Build image') {
         steps {
            sh(script: """
               cd azure-vote
               docker build -t azure-vote .
               docker images -a
            """)
         }
      }
   }
}