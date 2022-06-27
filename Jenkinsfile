node 
  {	  	          
	stage ('Workspace Cleanup') {
	  cleanWs()	                          
	}
	stage('Code Checkout')
	{
        git url: 'https://github.com/sahana179/FastAPI.git', branch: 'master' 
	}         
    stage('Build'){        	   
            sh label: '', script: '''                                        
            docker build -t sahanas179/fast-api:$BUILD_NUMBER .           
            '''  
            echo "Build Succcessful"     	    
    }
    stage('Push the Docker image') {                     
            withCredentials([usernamePassword( credentialsId: 'docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){                                                              
                sh label: '', script: '''                               
                docker login --username $USERNAME --password $PASSWORD
                docker push sahanas179/fast-api:$BUILD_NUMBER             
                docker rmi -f sahanas179/fast-api:$BUILD_NUMBER
                '''
          }

    }
    stage('Validate Helm Chart'){                             
        sh label: '', script: '''   
        cd helm        
        helm lint fastapi        
        '''          
     }  
     stage('Deploy to K8s'){     
      //  timeout(time: 10, unit: 'MINUTES') {
      //  input message: "Do you want to proceed for deployment?"
     //}                   
        sh label: '', script: '''   
        cd helm
        helm upgrade --install fastapi fastapi/ -f fastapi/values-dev.yaml --set image.tag=$BUILD_NUMBER' -n fa-dev
        '''          
     }
                    
}
