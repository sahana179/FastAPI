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
            docker build -t sahanas179/FastAPI:$BUILD_NUMBER .           
            '''  
            echo "Build Succcessful"     	    
    }
    stage('Push the Docker image'){        
            sh label: '', script: '''                                                   
            docker push sahanas179/FastAPI:$BUILD_NUMBER             
            docker rmi -f sahanas179/FastAPI:$BUILD_NUMBER
            '''                      
    }
    stage('Validate Helm Chart'){                             
        sh label: '', script: '''   
        cd helm
        sed -i 's/btag/'$BUILD_NUMBER'/g' values.tf
        helm validate        
        '''          
     }  
     stage('Deploy to K8s'){     
      //  timeout(time: 10, unit: 'MINUTES') {
      //  input message: "Do you want to proceed for deployment?"
     //}                   
        sh label: '', script: '''   
        cd helm
        helm upgrade --install FastAPI FastAPI/ -n fa-dev
        '''          
     }
                    
}
