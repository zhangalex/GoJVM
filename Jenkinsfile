pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script {
          def imgUrl = ECR_URL + "/nparks-poi-service"
          def imgVer = '1'
          def ecsCluster = "${ECS_CLUSTER}-lab"
          withAWS(credentials: AWS_CREDENTIAL_ID, region: AWS_REGION) {
            docker.image("releaseworks/awscli:latest").inside("--entrypoint \"\" -e AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY -e AWS_DEFAULT_REGION") {
              def tdJson = sh(returnStdout:true, script: "aws ecs describe-task-definition --task-definition ${TASK_DEF}").trim()

              def keys = ['family', 'taskRoleArn', 'executionRoleArn', 'networkMode', 'containerDefinitions', 'volumes', 'placementConstraints', 'requiresCompatibilities', 'cpu', 'memory', 'tags', 'pidMode', 'ipcMode', 'proxyConfiguration']
              def json = readJSON text: tdJson, returnPojo: true
              json = json.taskDefinition
              json.containerDefinitions.each {
                if (it.image.startsWith(imgUrl)) {
                  it.image = imgUrl + ":" + imgVer
                }

              }
              // echo json.containerDefinitions[0]

            }
          }
        }

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