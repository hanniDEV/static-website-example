
pipeline {

    environment {
        IMAGE_NAME = "webapp"
        IMAGE_TAG = "ajc-1.0"
        PRODUCTION = "yamen-ajc-prod-env"
        USERNAME = "yamen78"
        CONTAINER_NAME = "webapp"
        EC2_PRODUCTION_HOST="44.201.156.165"
	
	
	
    }






    agent none

    stages{




       stage ('Build Image'){
           agent any
           steps {
               script{
                   sh 'docker build -t $USERNAME/$IMAGE_NAME:$IMAGE_TAG .'
               }
           }
       }



       stage ('Run test container') {
           agent any
           steps {
               script{
                   sh '''
                       docker stop $CONTAINER_NAME || true
                       docker rm $CONTAINER_NAME || true
                       docker run --name $CONTAINER_NAME -d -e PORT=5000 -p 5000:80 $USERNAME/$IMAGE_NAME:$IMAGE_TAG
                       sleep 5
                   '''
               }
           }
       }

      

stage ('Test application') {
           agent any
           steps {
               script{
                   sh '''
                       curl http://localhost:5000 | grep -iq "dimension"
                   '''
               }
           }
       }

	
	    
      stage ('clean env and save artifact') {
           agent any
           environment{
               PASSWORD = credentials('dockerhub_password')
           }
           steps {
               script{
                   sh '''
                       docker login -u $USERNAME -p $PASSWORD
                       docker push $USERNAME/$IMAGE_NAME:$IMAGE_TAG
                       docker stop $CONTAINER_NAME || true
                       docker rm $CONTAINER_NAME || true
                       docker rmi $USERNAME/$IMAGE_NAME:$IMAGE_TAG
                   '''
               }
           }
       }

	    




    }
  
  
  
  
  



}
