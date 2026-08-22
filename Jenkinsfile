@Library('jShrLibs') _
pipeline {
    agent any

    environment {
        SERVICE       = "${env.BRANCH_NAME}"
        IMAGE_TAG     = "v${BUILD_NUMBER}"
        DOCKERHUB_NS  = "ozairkhan1"
        IMAGE         = "${DOCKERHUB_NS}/${SERVICE}:${IMAGE_TAG}"
        MANIFEST_REPO = "https://github.com/OzairKhan1/Kubernetes-ManifestFiles.git"
        MANIFEST_DIR  = "11-Microservices-Manifests"
    }

    stages {
	
	
	stage('Git Clone') {

            steps {

                gitClone("https://github.com/OzairKhan1/DevSecOps.git",SERVICE, "git-creds")

            }

        }
		
	stage('Build & Push') {

        steps {

            // sh "docker build -t ozairkhan1/${SERVICE}:${IMAGE_TAG} ."
            sh "docker tag  ozairkhan1/${SERVICE}:latest ozairkhan1/${SERVICE}:${IMAGE_TAG}"
                 
            dockerPush("ozairkhan1/${SERVICE}:${IMAGE_TAG}", "dockerHub-creds")

        }

    }

        // stage('SonarQube Analysis') {
        //     steps {
        //         withSonarQubeEnv('sonar-server') {
        //             sh '''
        //                 sonar-scanner \
        //                 -Dsonar.projectKey=${SERVICE} \
        //                 -Dsonar.sources=.
        //             '''
        //         }
        //     }
        // }

        // stage('Trivy FS Scan') {
        //     steps {
        //         sh '''
        //             trivy fs --severity HIGH,CRITICAL --exit-code 1 .
        //         '''
        //     }
        // }

        // stage('Trivy Image Scan') {
        //     steps {
        //         sh """
        //             trivy image --severity HIGH,CRITICAL --exit-code 1 ${IMAGE}
        //         """
        //     }
        // }


        stage('Update Manifest Repo') {
            steps {
                dir('manifests') {
                    gitClone("${MANIFEST_REPO}", "main", "git-creds")

                    sh """
                        sed -i "s#image: ${DOCKERHUB_NS}/${SERVICE}:.*#image: ${IMAGE}#g" ${MANIFEST_DIR}/${SERVICE}-deployment.yml
                    """

                    withCredentials([usernamePassword(
                        credentialsId: 'git-creds',
                        usernameVariable: 'GIT_USER',
                        passwordVariable: 'GIT_PASS'
                    )]) {
                        sh """
                            git config user.email "jenkins@ci.local"
                            git config user.name "jenkins-ci"
                            git add ${MANIFEST_DIR}/${SERVICE}-deployment.yml
                            git commit -m "Update ${SERVICE} image to ${IMAGE_TAG} (build ${BUILD_NUMBER})" || echo "No changes to commit"
                            git push https://${GIT_USER}:${GIT_PASS}@github.com/OzairKhan1/Kubernetes-ManifestFiles.git main
                        """
                    }
                }
            }
        }

        
    }
}
