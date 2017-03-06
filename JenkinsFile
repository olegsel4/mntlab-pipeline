node ('master') {
   
 stage('Preparation')
   {
   sh 'which java'
   sh 'echo $JAVA_HOME'
      echo BUILD_NUMBER
      echo WORKSPACE
    
    	echo "Getting Source:"
    	checkout([$class: 'GitSCM', branches: [[name: '*/aslesarenka']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/MNT-Lab/mntlab-pipeline']]])
         echo "Success!"
   }

  stage('Building code') 
   {
   		echo "Starting build"
   		sh 'chmod +x gradlew'
   		sh './gradlew build'
                echo "Success!"
   }

   stage('Testing') 
   {
   		echo "##########Testing##########"
   		parallel (
   		unit_tests: {sh './gradlew build'},
   		jacoco_tests: {sh './gradlew jacoco'},
   		cucumber_tests: {sh './gradlew cucumber'}
   		)
   }

 // stage('Packaging and Publishing results') 
 //  {
  //artefactName = sh (script: "basename ${WORKSPACE}" + '.jar', returnStdout: true) 
   //echo "Git committer email: ${artefactName}"
   	//	echo "Starting Packaging:"
   	//	build job: 'MNTLAB-aslesarenka-child1-build-job', parameters: [[$class: 'GitParameterValue', name: 'BRANCH_NAME', value: 'origin/yskrabkou'], string(name: 'WORKSPACE', value: "${WORKSPACE}")]
   		 
   	//	sh "cp build/libs/\$(basename \${WORKSPACE}).jar ."
   	
   	//	sh "tar czvf pipeline-aslesarenka-${BUILD_NUMBER}.tar.gz \$(basename \${WORKSPACE}).jar jobs.groovy Jenkinsfile"
   	//	archiveArtifacts artifacts: 'pipeline-aslesarenka-${BUILD_NUMBER}.tar.gz', excludes: null
 //               echo "Success!"
//}

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
