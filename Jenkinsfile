pipeline{

    agent any 

    environment{

        DOCKERHUB_USERNAME = "perarasan"
        APP_NAME = "Gitops"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = 'docker'
    }

    stages{

        stage('Clean workspace'){

            steps{
                script{

                    cleanWs()
                }
            }
        }

        stage('git SCM checkout'){

            steps{
                script{

                    git branch: 'main', url: 'https://github.com/perarasanvijay12/gitops_argocd_project.git'
                }
            }
        }

        stage('Docker Image build'){

            steps{
                script{

                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
         stage('Docker Image push'){

            steps{
                script{

                    docker.withRegistry('',REGISTRY_CREDS){
                        docker_image.push("$BUILD_NUMBER")
                        docker_image.push('latest')
                    }
                }
            }
        }
    }
}


