
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
                ls -r
                echo "***** 1 ******"
                apictl set --vcs-source-repo-path ./
                echo "***** 2 ******"
                apictl get envs
                echo "***** 3 ******"
                apictl login dev -u admin -p admin -k
                echo "***** 4 ******"
                ls -R /var/lib/jenkins
                apictl vcs deploy -e dev -k
                """
            }
        }
    }   
}
