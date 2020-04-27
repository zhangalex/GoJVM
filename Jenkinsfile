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
              withAWS(credentials: AWS_CREDENTIAL_ID, region: AWS_REGION) {
                def tdJson = sh(returnStdout:true, script: "aws ecs describe-task-definition --task-definition td-nparks-poi").trim()
                def json = new groovy.json.JsonSlurper().parseText(tdJson)
                def keys = ['family', 'taskRoleArn', 'executionRoleArn', 'networkMode', 'containerDefinitions', 'volumes', 'placementConstraints', 'requiresCompatibilities', 'cpu', 'memory', 'tags', 'pidMode', 'ipcMode', 'proxyConfiguration']

                json.get('taskDefinition').keySet().each {
                  def target = it
                  def i = keys.findIndexOf { it == target }
                  if (i == -1) {
                    json.remove(it)
                  }
                }

                //                def jsonStr = groovy.json.JsonOutput.toJson(json)
                def json2Input = readJSON text: json
                writeJSON file: 'output.json', json: json2Input

                sh 'cat output.json'

                //                println groovy.json.JsonOutput.toJson(json)
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