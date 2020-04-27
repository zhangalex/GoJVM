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
                docker.image("releaseworks/awscli:latest").inside("--entrypoint \"\" -e AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY -e AWS_DEFAULT_REGION") {
                  def tdJson = sh(returnStdout:true, script: "aws ecs describe-task-definition --task-definition td-nparks-poi").trim()

                  def keys = ['family', 'taskRoleArn', 'executionRoleArn', 'networkMode', 'containerDefinitions', 'volumes', 'placementConstraints', 'requiresCompatibilities', 'cpu', 'memory', 'tags', 'pidMode', 'ipcMode', 'proxyConfiguration']
                  def json = readJSON text: tdJson
                  json = json.get('taskDefinition')
                  json.keySet().each {
                    def target = it
                    def i = keys.findIndexOf { it == target }
                    if (i == -1) {
                      json.remove(it)
                    }
                  }
                  writeJSON file: 'new_td.json', json: json

                  sh "aws ecs register-task-definition --cli-input-json file://new_td.json"
                }
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
  environment {
    EMAIL_TO = 'alex.zhang@yumong.com'
    AWS_CREDENTIAL_ID = 'aws-credentials-username-password'
    ECR_URL = '853923275848.dkr.ecr.ap-southeast-1.amazonaws.com'
    AWS_REGION = 'ap-southeast-1'
    ECS_CLUSTER = 'cluster-nparks'
    ECS_SERVICE_NAME = 'svc-nparks-poi'
    TASK_DEF = 'td-nparks-poi'
  }
  triggers {
    githubPush()
  }
}