

pipeline {
  agent { 
  label 'ansible'
  }
  environment {
   AWS_EC2_PRIVATE_KEY=credentials('ec2-private-key') 
  }
  
  stages {
    
    //Get the Code from GitHub Repo
    stage('CheckOutCode'){
      steps{
        git credentialsId: '818fd7ea-10cd-44eb-b6ab-629c6cf30ebd', url: 'https://github.com/hindupursai/jekins-ansible-dynimc-inv.git'
      }
    }
     
    //Using Terrafrom can create the Servers
    
    //stage('CreateServers'){
      //steps{
       //sh "terraform  -chdir=terraformscripts init"
       //sh "terraform  -chdir=terraformscripts apply --auto-approve"
      //}
    //}
    
    //Run the playbook
    stage('RunPlaybook') {
      steps {
        sh "whoami"
        //List the dymaic inventory just for verification
        sh "ansible-inventory --graph -i inventory/aws_ec2.yaml"
        //Run playbook using dynamic inventory & limit exuection only fo tomcatservers.
        sh "ansible-playbook -i inventory/aws_ec2.yaml  playbooks/tomcat-setup.yaml -u ec2-user --private-key=$AWS_EC2_PRIVATE_KEY --limit tomcatservers --ssh-common-args='-o StrictHostKeyChecking=no'"
      }
    }
  
  }//stages closing
}//pipeline closing
