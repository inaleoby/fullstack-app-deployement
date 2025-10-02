
pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('DOCKER-HUB')  // ID des credentials dans Jenkins
        IMAGE_TAG = "v${BUILD_NUMBER}" // Tag dynamique basé sur le numéro de build (BUIL_NUMBER renvoit le numero du build)
        LASTEST_TAG = "latest"
        PORT = "5000"
        MONGO_URI = "mongodb://mongodb:27017/smartphoneDB"
        DELETE_CODE = "123"
    }
    stages {

        stage('BUILD IMAGES DOCKER') {
            
            parallel {
                stage('Build Frontend') {
                    steps {
                        dir('frontend') {
                            sh "docker build . -t espoir10/frontend:${IMAGE_TAG} -t espoir10/frontend:latest"
                    }
                }
            }

                stage('Build Backend') {
                    steps {
                        dir('backend') {
                            sh "docker build . -t espoir10/backend:${IMAGE_TAG} -t espoir10/backend:latest"
                    }
                }
            }
        }
    }

        
        stage('LOGIN TO DOCKER HUB') { 
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('PUSH IMAGES') {
            steps {
                sh """
                docker push espoir10/frontend:${IMAGE_TAG}
                docker push espoir10/frontend:${LASTEST_TAG}
                docker push espoir10/backend:${IMAGE_TAG}
                docker push espoir10/backend:${LASTEST_TAG}
                """
            }
        }

       stage('Deploy') {
            steps {
                    sh 'docker compose down'
                    sh 'docker compose up -d'
                   
        }

    }

post {
    success {
        emailext (
            subject: "✅ BUILD REUSSI - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """<html>
                        <body>
                            <p>Bonjour,</p>
                            <p>Le job <b>${env.JOB_NAME}</b> (build #${env.BUILD_NUMBER}) a été exécuté avec succès.</p>
                            <p>Consultez les logs ici : <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                        </body>
                     </html>""",
            to: 'obympeespoir@gmail.com, dangawa2000@gmail.com, oldpipa16@gmail.com, ndiayekhardiata2024@gmail.com',
            from: 'oldpipa16@gmail.com',
            replyTo: 'oldpipa16@gmail.com',
            mimeType: 'text/html'
        )
    }

    failure {
        emailext (
            subject: "❌ BUILD ECHOUE - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """<html>
                        <body>
                            <p>Bonjour,</p>
                            <p>Le job <b>${env.JOB_NAME}</b> (build #${env.BUILD_NUMBER}) a échoué.</p>
                            <p>Consultez les logs ici : <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                        </body>
                     </html>""",
            to: 'obympeespoir@gmail.com',
            from: 'oldpipa16@gmail.com',
            replyTo: 'oldpipa16@gmail.com',
            mimeType: 'text/html'
        )
    }

    always {
        // Optionnel : nettoyage ou déconnexion Docker
        sh 'docker logout'
    }
    
    }

}
