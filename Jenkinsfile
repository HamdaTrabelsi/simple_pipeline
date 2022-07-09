
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
                apictl get envs
                apictl login dev -u admin -p admin -k
                echo "**************"
                apictl vcs status -e dev
                echo "**************"
                apictl vcs deploy -e dev -k
                """
            }
        }
    }   
}
