
pipeline {

    agent {
        node {
            label 'node'
        }
    }
    environment { 
        PATH = "/root/apictl:$PATH"
    }
    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '16', 
                    numToKeepStr: '10'
            )
    }

    stages {

        stage('Setup Environment for APICTL') {
            steps {
                sh """#!/bin/bash
                ENVCOUNT=\$(apictl get envs --format {{.}} | wc -l)
                if [ "\$ENVCOUNT" == "0" ]; then
                    apictl add env dev  --apim https://am.wso2.com  --registration https://am.wso2.com  --token https://websub.am.wso2.com/token -k
                fi
                """
            }
        }

        stage('Deploy APIs To "Dev" Environment') {
            steps {
                sh """
                echo "***** export set *****"
                #apictl set --export-directory /var/lib/jenkins/.wso2apictl/exported/apis/
                echo "***** deploy set *****"
                apictl set --vcs-deployment-repo-path /var/lib/jenkins/.wso2apictl/exported/apis/
                echo "***** rmdir *****"
                rmdir deploy
                #mkdir deploy
                ls -r
                echo "***** 1 ******"
                apictl set --vcs-source-repo-path ./
                echo ***** current *****
                ls
                echo "***** 2 ******"
                apictl get envs
                echo "***** 3 ******"
                apictl login dev -u admin -p admin -k
                echo "***** 4 ******"
                #ls /var/lib/jenkins/.wso2apictl/exported/apis
                apictl vcs deploy -e dev -k
                echo "***** 5 ******"
                #rmdir deploy
                """
            }
        }
    }   
}
