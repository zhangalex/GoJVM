pipeline {
  agent any
  stages {
when {
buildingTag()
}
    stage('Version') {
      steps {
        script {
          imgVer = TAG_NAME
          echo "version ${imgVer}"
        }

      }
    }

    stage('Build') {
      steps {
        script {
          echo 'build'
        }

      }
    }

    stage('Deploy') {
      options {
        timeout(time: 1, unit: 'DAYS')
      }
      input {
        message 'Deploy to Production? (Click "Proceed" to continue)'
      }
      steps {
        script {
          echo 'Deployed'
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
