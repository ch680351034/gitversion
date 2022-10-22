pipeline {

    agent any 
      parameters {
        gitParameter branchFilter: 'origin/(.*)', defaultValue: 'master', name: 'BRANCH', type: 'PT_BRANCH'
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
            
            options {
                 timeout(5)
            }
            steps {

               git branch: "${params.BRANCH}", url: 'https://github.com/ch680351034/gitversion.git'
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
