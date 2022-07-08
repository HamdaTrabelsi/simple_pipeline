
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
                    apictl add env live  --apim https://am.wso2.com  --registration https://am.wso2.com  --token https://websub.am.wso2.com/token
                fi
                """
            }
        }

        stage('Deploy APIs To "Live" Environment') {
            steps {
                sh """
                apictl get envs
                apictl login live -u admin -p admin
                apictl vcs deploy -e live
                """
            }
        }
    }   
}
