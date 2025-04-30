// CODE_CHANGE = getGitChanges()
pipeline {
    agent any
    // the tools is used for build artifacts
    // tools {
    //     // maven 'maven'
    //     // jdk 'jdk11'
    // } 

    parameters {
        string(name: 'VERSION', defaultValue: '', description: 'version to deploy on prod')
        choice(name: 'VERSION', choices: ['1.0.0', '1.0.1', '1.0.2'], description: 'version to deploy on prod')
        booleanParam(name: 'executeTests', defaultValue: true, description:'code change or not')
    }
    // definition personal variable
    environment {
        // this is a func provided by jenkins and use for connect to the prod server to get artifacts
        SERVER_CREDENTIALS = credentials('jenkins-example')
        NEW_VERSION = '1.0.1'
    }
    stages {
        stage('build') {
            steps {
                echo 'building application'
                // if use groovy , it may use ''
                echo "new version is ${NEW_VERSION}"
                sh 'mvn install'
            }
        }
        stage('test') {
            when {
                expression {
                    BRANCH_NAME == 'main' && CODE_CHANGE == true
                }
            }
            // when close only can appear once in one stage bracket
            // when {
            //     expression {
            //         params.executeTests == true
            //     }
            // }
            steps {
                echo 'testing application'
            }
        }
        stage('prod') {
            steps {
                echo 'running applicationf'
                // echo "deploying with credentials ${SERVER_CREDENTIALS}"
                // sh "${SERVER_CREDENTIALS}"
                // or use withCredentials
                withCredentials([
                    usernamePassword(
                        credentialsId: 'jenkins-example',
                        usernameVariable: USER,
                        passwordVariable: PWD
                    )
                ]){
                    sh "some script ${USER} ${PWD}"
                }
                echo "deploying version ${params.VERSION}"
            }
        }
    }
    post{
        always{
            // cleanup actions & send notification
            echo 'pipeline completed'
        }
        success{
            echo 'pipeline success'
        }
        failure{
            echo 'pipeline failed'
        }
    }
}