pipeline {
   agent any

   environment {
      KUBECONFIG = 'C:\ProgramData\Jenkins\.jenkins\workspace\NamespaceWordPress'
   }

   stages {
      stage('Namespace') {
            steps {
               script {
                  def namespaceExists = sh(script: 'kubectl get namespace wp', returnStatus: true) == 0
                  if (namespaceExists) {
                        echo 'Namespace wp exists'
                        return
                  } else {
                        echo 'Creating wp namespace'
                        sh 'kubectl create namespace wp'
                  }
               }
            }
      }

      stage('Helm') {
            steps {
               script {
                  sh 'helm dependency build ./bitnami/wordpress'
                  sh 'helm upgrade --install final-project-wp-scalefocus ./bitnami/wordpress -n wp'
               }
            }
      }
   }
}
