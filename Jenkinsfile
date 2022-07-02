node 
  {	  	          
	stage ('Workspace Cleanup') {
	  cleanWs()	                          
	}
	stage('Code Checkout')
	{
        git url: 'https://github.com/sahana179/FastAPI.git', branch: 'develop' 
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
    /*
    stage('Validate Helm Chart'){                             
        sh label: '', script: '''   
        cd helm        
        helm lint fastapi        
        '''          
     }  
     stage('Deploy to K8s'){                              
        sh label: '', script: '''   
        cd helm
        helm upgrade --install fastapi fastapi/ -f fastapi/values-dev.yaml --set image.tag=$BUILD_NUMBER -n fa-dev
        '''          
     }
     */
     stage('    '){

         withCredentials([usernamePassword( credentialsId: 'git', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]){                                                              
                sh label: '', script: '''                               
                git clone https://github.com/sahana179/helm-chart.git
                ls
                cd helm-chart
                ls
                yq eval '.image.tag = $BUILD_NUMBER' -i fastapi/values-dev.yaml
                git add .
                git commit -m "Triggered Build"
                git push https://$GIT_USERNAME:@GIT_PASSWORD@github.com/sahana179/helm-chart.git
                '''
          }        
     }
                    
}
