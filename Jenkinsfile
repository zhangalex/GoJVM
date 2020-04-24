pipeline {
  agent any
  stages {
    stage('a1') {
      parallel {
        stage('a1') {
          steps {
            echo 'a1'
            script {
              pomFiles = findFiles(glob: '**/*/pom.xml')


              for (i = 0; i < pomFiles.length; i++) {
                pomFile = pomFiles[i].path;
                echo pomFile
              }
            }

          }
        }

        stage('b1') {
          steps {
            echo 'b1'
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
      steps {
        echo 'd1'
      }
    }

  }
}