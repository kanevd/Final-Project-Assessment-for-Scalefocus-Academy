pipeline {
  agent any

  stages {
    stage('Namespace Check') {
      steps {
        script {
          try {
            def nsExists = bat(returnStatus: true, script: 'kubectl get namespace wp')
            if (nsExists == 0) {
              echo "Namespace wp already exists"
            } else {
              echo "Creating namespace wp"
              writeFile file: 'namespace.yaml', text: '''
              apiVersion: v1
              kind: Namespace
              metadata:
                name: wp
              '''
              bat 'kubectl apply -f namespace.yaml'
            }
          } catch (Exception e) {
            echo "Error checking/creating namespace wp: ${e.getMessage()}"
          }
        }
      }
    }

    stage('Helm Install') {
      steps {
        script {
          try {
            bat 'helm dependency build bitnami/wordpress'
            def chartExists = bat(returnStatus: true, script: 'helm list -q wp --namespace wp')
            if (chartExists == 0) {
              echo "Chart wp already exists"
            } else {
              echo "Installing chart wp"
              bat 'helm install final-project-wp-scalefocus bitnami/wordpress --namespace wp -f bitnami/wordpress/values.yaml --set service.type=ClusterIP'
            }
          } catch (Exception e) {
            echo "Error installing Helm chart: ${e.getMessage()}"
          }
        }
      }
    }
  }
}
