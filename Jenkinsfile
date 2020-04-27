pipeline {
  agent any
  stages {
    stage('a1') {
      parallel {
        stage('a1') {
          steps {
            echo 'a1'
            script {
              pomFiles = findFiles(glob: '**/pom.xml')


              for (i = 0; i < pomFiles.length; i++) {
                pomFile = pomFiles[i].path;
                echo pomFile
              }
            }

          }
        }

        stage('b1') {
          steps {
            configFileProvider([configFile(fileId: 'td-nparks-poi-service', variable: 'TD_FILE_PATH')]) {
              sh 'cat $TD_FILE_PATH'
            }

            sh 'curl -s \'https://api.github.com/users/lambda\' | jq -r \'.name\''
          }
        }

        stage('c1') {
          steps {
            echo 'c1'
          }
        }

      }
    }

    stage('d1') {
      input {
        message 'Deploy to PROD? (Click "Proceed" to continue)'
      }
      steps {
        echo 'd1'
      }
    }

  }
  triggers {
    githubPush()
  }
}