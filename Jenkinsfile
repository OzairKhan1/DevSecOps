@Library('jShrLibs') _

pipeline {
    agent any

    stages {
        stage('Kubernetes Authentication') {
            steps {
                withCredentials([
                    file(
                        credentialsId: 'kubeconfig',
                        variable: 'KUBECONFIG'
                    )
                ]) {
                    sh '''
                        kubectl config current-context
                        kubectl get pods
                    '''
                }
            }
        }

    }
}
