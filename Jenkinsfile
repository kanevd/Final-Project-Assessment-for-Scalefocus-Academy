pipeline {
  agent any

  stages {
    stage('Namespace Check') {
      steps {
        script {
          try {
            def nsExists = sh(
              returnStatus: true,
              script: 'kubectl get namespace wp'
            )
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
              sh 'kubectl apply -f namespace.yaml'
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
            sh 'helm dependency build bitnami/wordpress'
            def chartExists = sh(
              returnStatus: true,
              script: 'helm list -q wp --namespace wp'
            )
            if (chartExists == 0) {
              echo "Chart wp already exists"
            } else {
              echo "Installing chart wp"
              sh 'helm install wp bitnami/wordpress --namespace wp -f bitnami/wordpress/values.yaml --set service.type=ClusterIP'
            }
          } catch (Exception e) {
            echo "Error installing Helm chart: ${e.getMessage()}"
          }
        }
      }
    }
  }
}
