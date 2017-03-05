node('master')
{
    tool name: 'java8', type: 'jdk'
    tool name: 'gradle3.3', type: 'gradle'
    
    stage ('Preparation (Checking out).')
    {
	    git url:'https://github.com/MNT-Lab/mntlab-pipeline.git', branch:'pheraska'
    }
	
    stage ('Building code.')
    {
	sh '''
           export PATH=$PATH:${JENKINS_HOME}/tools/hudson.plugins.gradle.GradleInstallation/gradle3.3/bin/
           export JAVA_HOME=${JENKINS_HOME}/tools/hudson.model.JDK/java8/
           gradle build
           '''
    }
	
    stage 'Testing.'
    {
        parallel JUnit: 
	{
	    Test:
	    {
	        sh 'gradle test'
	    }
        }
    }
	
    stage 'Triggering job and fetching artefact after finishing.'

    stage 'Packaging and Publishing results.'
	
    stage 'Asking for manual approval.'

    stage 'Deployment.'

    stage 'Sending status.'
}
