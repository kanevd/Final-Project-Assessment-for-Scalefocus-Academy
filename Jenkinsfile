pipeline {
   agent any

   environment {
      KUBECONFIG = 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\NamespaceWordPress'
   }

   stages {
      stage('Preparation') {
         steps {
            git branch: 'main', url: 'https://github.com/kanevd/Final-Project-Assessment-for-Scalefocus-Academy.git'
         }
      }

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
               bat 'helm dependency build .\\bitnami\\wordpress'
               bat 'helm upgrade --install final-project-wp-scalefocus .\\bitnami\\wordpress -n wp'
            }
         }
      }
   }
}
