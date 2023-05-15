pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/kanevd/Final-Project-Assessment-for-Scalefocus-Academy.git'
            }
        }

        stage('Create Namespace') {
            steps {
                script {
                    try {
                        sh "kubectl create namespace wp --kubeconfig=\"${WORKSPACE}/kubeconfig.yaml\""
                    } catch (Exception e) {
                        echo "Namespace 'wp' already exists, skipping..."
                    }
                }
            }
        }

        stage('Deploy WordPress') {
            steps {
                script {
                    try {
                        sh "helm install final-project-wp-scalefocus stable/wordpress -n wp --kubeconfig=\"${WORKSPACE}/kubeconfig.yaml\" --set wordpressUsername=admin,wordpressPassword=password,wordpressEmail=admin@example.com,persistence.enabled=true,persistence.storageClass=standard,persistence.accessMode=ReadWriteOnce"
                    } catch (Exception e) {
                        error "Error deploying WordPress: ${e.message}"
                    }
                }
            }
        }
    }
}
