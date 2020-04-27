pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script {
          def ecsCluster = ${ECS_CLUSTER}-lab
          withAWS(credentials: AWS_CREDENTIAL_ID, region: AWS_REGION) {
            docker.image("releaseworks/awscli:latest").inside("--entrypoint \"\" -e AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY -e AWS_DEFAULT_REGION") {
              def tdJson = sh(returnStdout:true, script: "aws ecs describe-task-definition --task-definition ${TASK_DEF}").trim()

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

              def newTdJsonStr = sh(returnStdout:true, script: "aws ecs describe-task-definition --task-definition ${TASK_DEF}").trim()
              def newTdJson = readJSON text: newTdJsonStr
              def taskRevision = newTdJson.taskDefinition.revision

              def svcJsonStr = sh(returnStdout:true, script: "aws ecs describe-services --cluster ${ecsCluster}--services ${ECS_SERVICE_NAME}").trim()
              def svcJson = readJSON text: svcJsonStr
              def desiredCount = svcJson.desiredCount
              if (desiredCount == null){
                desiredCount=1
              }

              sh "aws ecs update-service --cluster ${ecsCluster} --service ${ECS_SERVICE_NAME} --task-definition ${TASK_DEF}:${taskRevision} --desired-count ${desiredCount}"
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