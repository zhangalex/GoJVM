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
            script {
              configFileProvider([configFile(fileId: 'td-nparks-poi-service', variable: 'TD_FILE_PATH')]) {
                def tdJson = readFile(TD_FILE_PATH)
                def json = new groovy.json.JsonSlurper().parseText(tdJson)

                json.keySet().each {echo it}

                println groovy.json.JsonOutput.toJson(json)
              }
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