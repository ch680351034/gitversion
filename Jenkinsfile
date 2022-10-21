pipeline {

    agent any 
parameters {
  gitParameter branch: '', branchFilter: '.*', defaultValue: 'origin/master', name: 'BRANCH', quickFilterEnabled: false, selectedValue: 'NONE', sortMode: 'NONE', tagFilter: '*', type: 'GitParameterDefinition', useRepository: 'https://github.com/ch680351034/gitversion.git'
}

    

    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '7', 
                    numToKeepStr: '5'
            )
    }

    stages {
        
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                sh """
                echo "Cleaned Up Workspace For Project"
                """
            }
        }

        stage('Code Checkout') {
            steps {

                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: 'develop']], 
                    userRemoteConfigs: [[url: 'https://github.com/ch680351034/gitversion.git']]
                ])

               //sh 'version=$(gitversion | jq -r '.MajorMinorPatch')'
                sh 'gitversion > version.json'
                sh 'cat version.json'
               // sh "version=$(jq -r '.MajorMinorPatch' version.json)"
                script {
                    def props = readJSON file: 'version.json'
                    println "${props.SemVer}"
                    currentBuild.displayName = props.SemVer
                }
                
                //echo $version
            }
        }

        stage(' Unit Testing') {
            steps {
                sh """
                echo "Running Unit Tests"
                """
            }
        }

        stage('Code Analysis') {
            steps {
                sh """
                echo "Running Code Analysis"
                """
            }
        }

        stage('Build Deploy Code') {
            when {
                branch 'develop'
            }
            steps {
                sh """
                echo "Building Artifact"
                """

                sh """
                echo "Deploying Code"
                """
            }
        }

    }   
}
