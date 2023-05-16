pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/kanevd/Final-Project-Assessment-for-Scalefocus-Academy.git'
            }
        }

        stage('Create Namespace') {
            steps {
                script {
                    try {
                        bat 'kubectl create namespace wp --kubeconfig="C:\\ProgramData\\Jenkins\\.kube\\config"'
                    } catch (err) {
                        echo "Namespace 'wp' already exists, skipping..."
                    }
                }
            }
        }

        stage('Deploy WordPress') {
            steps {
                script {
                    bat '"C:\\Users\\dkane\\Downloads\\helm-v3.12.0-windows-amd64\\windows-amd64\\helm.exe" repo add bitnami https://charts.bitnami.com/bitnami --kubeconfig="C:\\ProgramData\\Jenkins\\.kube\\config"'
                    bat '"C:\\Users\\dkane\\Downloads\\helm-v3.12.0-windows-amd64\\windows-amd64\\helm.exe" install final-project-wp-scalefocus bitnami/wordpress -n wp --kubeconfig="C:\\ProgramData\\Jenkins\\.kube\\config" --set wordpressUsername=admin,wordpressPassword=password,wordpressEmail=admin@example.com,persistence.enabled=true,persistence.storageClass=standard,persistence.accessMode=ReadWriteOnce'
                }
            }
        }

        stage('Port Forwarding') {
            steps {
                script {
                    try {
                        bat 'kubectl port-forward service/final-project-wp-scalefocus 8080:80 --kubeconfig="C:\\ProgramData\\Jenkins\\.kube\\config"'
                    } catch (err) {
                        echo "Failed to start port forwarding: ${err}"
                    }
                }
            }
        }
    }
}
