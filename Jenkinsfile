pipeline {
    agent any

    environment {
        HELM_PATH = "C:\\Users\\dkane\\Downloads\\helm-v3.12.0-windows-amd64\\windows-amd64\\helm.exe"
        KUBECONFIG_PATH = "${WORKSPACE}\\kubeconfig.yaml"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Create Namespace') {
            steps {
                script {
                    try {
                        bat "kubectl create namespace wp --kubeconfig=\"${KUBECONFIG_PATH}\""
                    } catch (Exception e) {
                        echo "Namespace 'wp' already exists, skipping..."
                    }
                }
            }
        }

        stage('Clone Repository') {
            steps {
                script {
                    bat "git clone https://github.com/kanevd/Final-Project-Assessment-for-Scalefocus-Academy.git"
                }
            }
        }

        stage('Deploy WordPress') {
            steps {
                script {
                    try {
                        bat "\"${HELM_PATH}\" install final-project-wp-scalefocus stable/wordpress -n wp --kubeconfig=\"${KUBECONFIG_PATH}\" --set wordpressUsername=admin,wordpressPassword=password,wordpressEmail=admin@example.com,persistence.enabled=true,persistence.storageClass=standard,persistence.accessMode=ReadWriteOnce"
                    } catch (Exception e) {
                        error "Error deploying WordPress: ${e.message}"
                    }
                }
            }
        }

        stage('Port Forwarding') {
            steps {
                script {
                    try {
                        bat "kubectl port-forward svc/final-project-wp-scalefocus-wordpress 8080:80 --kubeconfig=\"${KUBECONFIG_PATH}\""
                    } catch (Exception e) {
                        error "Error setting up port forwarding: ${e.message}"
                    }
                }
            }
        }

        // Add more stages as needed
    }
}
