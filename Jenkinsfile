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
        
    }

	post {
        success {
            build job: 'Update-Manifests',
                  parameters: [
                      string(name: 'IMAGE_TAG', value: "${IMAGE_TAG}"),
					  string(name: 'MANIFEST_REPO', value: "${MANIFEST_REPO}"),
					  string(name: 'DOCKERHUB_NS', value: "${DOCKERHUB_NS}"),
					  string(name: 'SERVICE', value: "${SERVICE}"),
					  string(name: 'IMAGE', value: "${IMAGE}"),
					  string(name: 'MANIFEST_DIR', value: "${MANIFEST_DIR}")
                  ]
        }
    }

	
}
