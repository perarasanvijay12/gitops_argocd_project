pipeline{

    agent any 

    environment{

        DOCKERHUB_USERNAME = "perarasan"
        APP_NAME = "gitops"
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
        stage('Docker Remove Image'){

            steps{
                script{

                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"

                    }
                }
            }
        stage('Updating K8s deployment file'){

            steps{
                script{

                    sh """
                    cat deployment.yml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
                    cat deployment.yml
 
                    """

                    }
                }
            }
        stage('Push the changed deployment file to Git'){

            steps{
                script{

                    sh """
                    git config --global user.name "perarasanvijay12"
                    git config --global user.email "perarasanvijay5000@gmail.com"   
                    git add deployment.yml
                    git commit -m "updated deployment file"
                    git remote set-url origin https://{perarasanvijay12}:{Perarasanvijay@123}@github.com/{username}/project.git 
                    """

                    }
            }
        }
    }

}

