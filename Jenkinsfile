pipeline {
  agent any

  stages {
    stage('Namespace Check') {
      steps {
        script {
          try {
            def nsExists = bat(
              returnStatus: true,
              script: 'kubectl get namespace wp'
            )
            if (nsExists == 0) {
              echo "Namespace wp already exists"
            } else {
              echo "Creating namespace wp"
              bat 'kubectl create namespace wp'
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
            def chartExists = bat(
              returnStatus: true,
              script: 'helm list -q wp --namespace wp'
            )
            if (chartExists == 0) {
              echo "Chart wp already exists"
            } else {
              echo "Installing chart wp"
              bat 'helm install wp bitnami/wordpress --namespace wp -f bitnami/wordpress/values.yaml --set service.type=ClusterIP'
            }
          } catch (Exception e) {
            echo "Error installing Helm chart: ${e.getMessage()}"
          }
        }
      }
    }

    stage('Wait for Pod Ready') {
      steps {
        script {
          try {
            def podName = sh(
              returnStdout: true,
              script: 'kubectl get pod -l app.kubernetes.io/name=wordpress -n wp -o jsonpath="{.items[0].metadata.name}"'
            ).trim()
        
            if (!podName.empty) {
              echo "WordPress pod is ready: $podName"
              
              def portForwardCommand = "kubectl port-forward -n wp pod/$podName 8090:80"
              
              bat portForwardCommand
            } else {
              error "WordPress pod is not ready"
            }
          } catch (Exception e) {
            error "Error checking WordPress pod status: ${e.getMessage()}"
          }
        }
      }
    }

    stage('Port Forward') {
      steps {
        script {
          try {
            bat "kubectl port-forward -n wp pod/$podName 8090:80"
          } catch (Exception e) {
            error "Error port forwarding: ${e.getMessage()}"
          }
        }
      }
    }
  }
}
