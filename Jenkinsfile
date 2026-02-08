pipeline {
  agent any

  options {
    timestamps()
  }

  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Checks Paralelos') {
      parallel {

        stage('Pruebas de SAST') {
          steps {
            sh 'echo "Ejecución de pruebas de SAST"'
          }
        }

        stage('Imprimir Env') {
          steps {
            sh 'echo "WORKSPACE=$WORKSPACE"'
          }
        }

      }
    }

    stage('Configurar archivo') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'Credentials_DevOps', usernameVariable: 'user', passwordVariable: 'password')]) {
          sh '''
            cat > credentials.ini <<EOF
[credentials]
user=${user}
password=${password}
EOF
          '''
        }

        archiveArtifacts artifacts: 'credentials.ini', fingerprint: true
      }
    }

    stage('Build') {
      steps {
        sh 'docker build -t devops_ws .'
      }
    }
  }
}
