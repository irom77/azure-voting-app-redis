pipeline {
   agent any
    environment {
        PATH = "$PATH:/usr/local/bin"
    }
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
               # ./scripts/test_container.ps1
            """)
         }
         /*post {
            success {
               echo "App started successfully :)"
            }
            failure {
               echo "App failed to start :("
            }
         }*/
      }
      stage('Run Tests') {
         steps {
            sh(script: """
               # find / -name pytest -type f 2>/dev/null 
               which pytest
               # pytest ./tests/test_sample.py
            """)
         }
      }
      stage('Stop test app') {
         steps {
            pwsh(script: """
               docker-compose down
            """)
         }
      }
   }
}
