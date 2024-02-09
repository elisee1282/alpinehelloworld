pipeline { 
  agent any

  parameters { 
    string(name: 'IMAGE_NAME', defaultValue: 'alpinehelloworld', description: 'le nom de mon image docker')
    string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'le tag de mon image')
    string(name: 'DOCKERHUB_ID', defaultValue: 'ebj288510', description: 'id docker hub')     
             }

  stages {

      stage('Build image') { 
        step  { 
          sh 'docker build -t ${DOCKERHUB_ID}/${IMAGE_NAME}:${IMAGE_TAG}'
                       }
          }

      stage('Delete old Container') { 
        step  { 
          sh '''docker rm Webapp --force

                sleep 5'''
                       }
          }

      stage('Instanciation nouveau container de test') { 
        step  { 
          sh '''docker run -d -p 80:5000 -e PORT=5000 --name Webapp ${DOCKERHUB_ID}/${IMAGE_NAME}:${IMAGE_TAG}

                sleep 8'''
                       }
          }

      stage('Curl Web du nouveau container de test') { 
        step  { 
          sh '''curl http://192.168.152.150:80 | grep -q "Hello world!"

                sleep 5'''
                       }
          }

      stage('Delete du container') { 
        step  { 
          sh '''docker rm Webapp --force

                sleep 5'''
                       }
          }

      stage('Connexion et Push image sur docker hub') { 
        step  { 
        withCredentials([usernamePassword(
        credentialsId: 'dockerhub-credentials',
        passwordVariable: 'DOCKERHUB_PASSWORD',
        usernameVariable: 'DOCKERHUB_USER'
    )]) {
        sh """
         docker login -u '$DOCKERHUB_USER' -p '$DOCKERHUB_PASSWORD'
         docker push ${DOCKERHUB_ID}/${IMAGE_NAME}:${IMAGE_TAG}
        """ 
    }
                       }
          }





          }
          }
