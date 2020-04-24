pipeline {
  agent any
  stages {
    stage('a1') {
      parallel {
        stage('a1') {
          steps {
            echo 'a1'
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