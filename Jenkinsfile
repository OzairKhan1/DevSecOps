@Library('jShrLibs') _

pipeline {

    agent any

    environment {

        IMAGE_TAG = "v${BUILD_NUMBER}"

        SERVICE   = "checkoutservice"
    }

    stages {

        stage('Git Clone') {

            steps {

                gitClone("https://github.com/OzairKhan1/DevSecOps.git", env.BRANCH_NAME, "git-creds")

            }

        }

        stage('Build & Push') {

            steps {

                // sh "docker build -t ozairkhan1/${SERVICE}:${IMAGE_TAG} ."
                 sh "docker tag  ozairkhan1/${SERVICE}:latest ozairkhan1/${SERVICE}:${IMAGE_TAG}"
                dockerPush("ozairkhan1/${SERVICE}:${IMAGE_TAG}", "dockerHub-creds")

            }

        }

    }

}
