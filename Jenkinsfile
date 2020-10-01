pipeline {
    agent any
     environment {
           SG_CLIENT_ID = credentials("SG_CLIENT_ID")
           SG_SECRET_KEY = credentials("SG_SECRET_KEY")
	   CHKP_CLOUDGUARD_ID = credentials("CHKP_CLOUDGUARD_ID")
	   CHKP_CLOUDGUARD_SECRET = credentials("CHKP_CLOUDGUARD_SECRET")
           }

   stages {

     stage('Checkout Terraform Files to Deploy IaC') {
      steps {
        checkout scm
       }

     }
   
	   
     stage('Shifleft SAST Terraform IaC-Assessment') {
	   
     steps {
	     echo 'Running Shiftleft Iac Assessment'
	       sh 'shiftleft iac-assessment -D -r 190928 -s critical .'
		   }
	   }
	   
	   
         stage('Code Approval Request') {
     
           steps {
             script {
               def userInput = input(id: 'confirm', message: 'Do you Approve to use this code?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Approve Code to Proceed', name: 'approve'] ])
              }
            }
          }


    stage('Terraform Init') {
       agent {
               docker { image 'checkpoint/shifleft'
                         args '--entrypoint=' }
                       }
        steps {
           withAWS(credentials: 'awscreds', region: 'us-east-1'){
               
             sh "terraform init --upgrade && terraform plan"
            
            }
         }
     }  
       
   stage('Terraform Plan Approval Request') {
      steps {
        script {
          def userInput = input(id: 'confirm', message: 'Do you Approve the Terraform Plan?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Confirm to Approve Terraform Plan', name: 'approve'] ])
        }
      }
    }
        
   stage('Deploy with Terraform Apply') {
       agent {
               docker { image 'dhouari/devsecops'
                         args '--entrypoint=' }
                       }
        steps {
          withAWS(credentials: 'awscreds', region: 'us-east-1'){
             
             sh "terraform apply --auto-approve"
            
          }
       }
    }

  }

 
	  

}
