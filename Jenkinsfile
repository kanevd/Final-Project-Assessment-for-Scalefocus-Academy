pipeline {
    agent any

    stages {
        stage('Fetch Dependencies') {
            steps {
                bat '''
                    mkdir charts
                    cd charts

                    :: Download and extract memcached chart
                    curl -LO https://charts.bitnami.com/bitnami/memcached-5.2.1.tgz
                    tar -xf memcached-5.2.1.tgz
                    del memcached-5.2.1.tgz

                    :: Download and extract mariadb chart
                    curl -LO https://charts.bitnami.com/bitnami/mariadb-10.5.12.tgz
                    tar -xf mariadb-10.5.12.tgz
                    del mariadb-10.5.12.tgz

                    :: Download and extract common chart
                    curl -LO https://charts.bitnami.com/bitnami/common-1.0.0.tgz
                    tar -xf common-1.0.0.tgz
                    del common-1.0.0.tgz

                    :: Go back to the project root directory
                    cd ..
                '''
            }
        }

        stage('Build and Deploy') {
            steps {
                script {
                    bat '''
                        :: Add helm repository
                        helm repo add bitnami https://charts.bitnami.com/bitnami --kubeconfig="C:\\ProgramData\\Jenkins\\.kube\\config"

                        :: Update helm repositories
                        helm repo update --kubeconfig="C:\\ProgramData\\Jenkins\\.kube\\config"

                        :: Perform Helm chart dependency build
                        helm dependency build . --kubeconfig="C:\\ProgramData\\Jenkins\\.kube\\config"

                        :: Install the WordPress chart
                        helm install final-project-wp-scalefocus bitnami/wordpress -n wp --kubeconfig="C:\\ProgramData\\Jenkins\\.kube\\config" --set wordpressUsername=admin,wordpressPassword=password,wordpressEmail=admin@example.com,persistence.enabled=true,persistence.storageClass=standard,persistence.accessMode=ReadWriteOnce
                    '''
                }
            }
        }

        stage('Port Forwarding') {
            steps {
                bat '''
                    :: Perform port forwarding for accessing WordPress
                    kubectl port-forward svc/final-project-wp-scalefocus-wordpress 8080:80 --kubeconfig="C:\\ProgramData\\Jenkins\\.kube\\config"
                '''
            }
        }
    }
}
