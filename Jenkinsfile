pipeline {
  agent any
  stages {
    stage('a1') {
      parallel {
        stage('a1') {
          steps {
            echo 'a1'
            script {
              def jsonStr = '{"a": 1, "b": [{"c": 3, "d": 4}]}}'
              def json = new JsonSlurper().parseText(jsonStr)
              // XXX: first "de-array" `b`
              json.b = json.b.first()
              // next remove `c` from it
              json.b.remove('c')
              println JsonOutput.toJson(json)

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