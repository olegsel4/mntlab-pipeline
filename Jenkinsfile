node  {
   stage('Preparation')
   {
    	echo "Start"
    	checkout([$class: 'GitSCM', branches: [[name: '*/yskrabkou']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/MNT-Lab/mntlab-pipeline']]])
   }

  stage('Building code') 
   {
   		echo "Building"
   		sh 'chmod +x gradlew'
   		sh './gradlew build'
   }

   stage('Testing') 
   {
   		echo "Testing"
   }

   stage('Packaging and Publishing results') 
   {
   		echo "Deployment"
   }

    stage('Asking for manual approval') 
   {
   		echo "Checking"
   }

   stage('Deployment') 
   {
   		echo "Checking"
   }

   stage('Sending status') 
   {
   		echo "Checking"
   }


  }