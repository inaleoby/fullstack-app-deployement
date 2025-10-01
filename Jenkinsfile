
pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('DOCKER-HUB')  // ID des credentials dans Jenkins
        IMAGE_TAG = "v${BUILD_NUMBER}" // Tag dynamique basé sur le numéro de build (BUIL_NUMBER renvoit le numero du build)
        LASTEST_TAG = "latest"
        PORT = "5000"
        MONGO_URI = "mongodb://mongo:27017/smartphoneDB"
        DELETE_CODE = "123"
    }
    stages {
        //stage('CLONER DEPOT') {
            //steps {
               // checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GIT-CRED', url: 'https://github.com/inaleoby/fullstack-app-deployement.git']])
            //}
        //}
        /*stage('BUILD IMAGES DOCKER') {
            steps {
                dir('Frontend') {
                    sh "docker build . -t espoir10/frontend:${IMAGE_TAG} -t espoir10/frontend:${LASTEST_TAG}"
                    }
                dir('Backend') {
                    sh "docker build . -t espoir10/backend:${IMAGE_TAG} -t espoir10/backend:${LASTEST_TAG}"
                    }
            }
        }*/
        
        stage('LOGIN TO DOCKER HUB') { 
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        
        /*stage('PUSH IMAGES') {
            steps {
                sh """
                docker push espoir10/frontend:${IMAGE_TAG}
                docker push espoir10/frontend:${LASTEST_TAG}
                docker push espoir10/backend:${IMAGE_TAG}
                docker push espoir10/backend:${LASTEST_TAG}
                """
            }
        }*/
      
        /*stage('DEPLOY') {
            steps {
                sh """
                
                export PORT=${PORT}
                export MONGO_URI=${MONGO_URI}
                export DELETE_CODE=${DELETE_CODE}

                docker compose down  # Arrête les anciens conteneurs s'ils existent
                docker compose up -d  # Démarre les nouveaux conteneurs en arrière-plan
                """
            }
        } */

        stage('Deploy') {
            steps {
                script {
                    // Génère .env à partir des variables Jenkins
                    writeFile file: '.env', text: """
                    PORT=${PORT}
                    MONGO_URI=${MONGO_URI}
                    DELETE_CODE=${DELETE_CODE}
                    """

                    // Relance docker-compose
                    sh 'docker compose down'
                    sh 'docker compose up -d'
                }
            }
        }

    }
    post {
        always {
            sh 'docker logout' // On se deconnecte du Hub
        }
    }
}
