node('host')
{
    tool name: 'java8', type: 'jdk'
    tool name: 'gradle3.3', type: 'gradle'
    def errorArray = []
    def jdkHome = tool 'java8'
    def gradleHome = tool 'gradle3.3'
    stage ('cleanup')
    {
        try
        {
            step([$class: 'WsCleanup'])
        }
        catch (error)
        {
            errorArray.push("ERROR: Cant cleanup workspace!")
        }        
    }
    
    stage ('Preparation (Checking out).')
    {
        try
        {
            git url:'https://github.com/MNT-Lab/mntlab-pipeline.git', branch:'pheraska'
            //git url:'https://github.com/kickman2l/test-jenkins.git', branch:'master'
        }
        catch (error)
        {
            errorArray.push("ERROR: Cant clone from git!")
        }
    }

    stage ('Building code.')
    {
        try
        {
            sh '''
            export PATH=$PATH:${gradleHome}/bin/
            export JAVA_HOME=$PATH:$jdkHome
            gradle build
            ''';
        }
        catch (error)
        {
            errorArray.push("ERROR: Cant build with gradle!")
        }
    }

    stage ('Testing.')
    {
        try
        {
            parallel JUnit:
            {
                sh '''
                export PATH=$PATH:${gradleHome}/bin/
                export JAVA_HOME=$PATH:$jdkHome
                gradle test
                ''';
            },
            Jacoco:
            {
                sh '''
                export PATH=$PATH:${gradleHome}/bin/
                export JAVA_HOME=$PATH:$jdkHome
                gradle cucumber
                ''';
            },
            Cucumber:
            {
            sh '''
                export PATH=$PATH:${gradleHome}/bin/
                export JAVA_HOME=$PATH:$jdkHome
                gradle jacoco
                ''';
            }
        }
        catch (error)
        {
            errorArray.push("ERROR: Something goes wrong with tests!")
        }
    }
    
    stage ('Triggering job and fetching artefact after finishing.')
    {
        try
        {
            build job: 'kickman', parameters: [[$class: 'StringParameterValue', name: 'BRANCH_NAME', value: "${env.BRANCH_NAME}"]]
            step ([$class: 'CopyArtifact', projectName: 'kickman']);
        }
        catch (error)
        {
            errorArray.push("ERROR: Cant trigger other project!")
        }
    }
    
    stage ('Packaging and Publishing results.')
    {
        try
        {
            sh '''
            cp ${WORKSPACE}/build/libs/$(basename "$PWD").jar ${WORKSPACE}
            tar -zxvf pheraska_dsl_script.tar.gz jobs.groovy
            tar -czf pipeline-${BRANCH_NAME}-${BUILD_NUMBER}.tar.gz jobs.groovy Jenkinsfile $(basename "$PWD").jar
            ''';
            archiveArtifacts artifacts: 'pipeline-${BRANCH_NAME}-${BUILD_NUMBER}.tar.gz'
        }
        catch (error)
        {
            errorArray.push("ERROR: Cant create artifacts!")
        }
    }

    stage ('Asking for manual approval.')
    {
        try
        {
            timeout(time:3, unit:'MINUTES') 
            {
                input message:'Approve deployment?'
            }
        }
        catch (error)
        {
            errorArray.push("ERROR: Somet wrong with with approve!")
        }
    }
    
    stage ('Deployment.')
    {
        try
        {
            sh '''
            export JAVA_HOME=$PATH:$jdkHome
            java -jar $(basename "$PWD").jar
            '''
        }
        catch (error)
        {
            errorArray.push("ERROR: Somet wrong with deployent!")
        }
    }
    
    stage ('Sending status.')
    {
        if ("${errorArray}" != "0")
        {
            echo "${errorArray}"
        }
        else
        {
            echo "SUCCESS: No errors found!"
        }
    }
}
