pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    parameters {
        choice(name: 'BRANCHES', choices: ['dev', 'master'], description: 'Chose a branch to build on.')
        string(name: 'GOALS', defaultValue: 'clean install', description: 'clean install is the default goal.')
    }
    stages{
        stage('SCM'){
            steps {
                git branch: "${params.BRANCHES}", url: 'https://github.com/AnasAnsar1/jenkins-git-integration.git'
                mail subject: "Build started",
                    body: "Build started on branch $env.BRANCH_NAME & on node $env.NODE_NAME",
                    to: "ansariianas78@gmail.com"
            }
        }
        stage("Artifactory configuration") {
            steps {
                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "JFROG_FEB23",
                    releaseRepo: "Jenkins-integration",
                    snapshotRepo: "Jenkins-snapshot"
                )
            }
        }
        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: 'MAVEN_3.6.3',
                    pom: 'pom.xml',
                    goals: "${params.GOALS}",
                    deployerId: "MAVEN_DEPLOYER"
                )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "JFROG_FEB23"
                )
            }
        }
    }
    post {
        always {
            mail subject: "Build completed",
                body: "Build Completed on branch $env.BRANCH_NAME & on node $env.NODE_NAME",
                to: "ansariianas78@gmail.com"
        }
        failure {
            mail subject: "Build Failed",
                body: "Build no $env.BUILD_NUMBER failed with ID $env.BUILD_ID on node $env.NODE_NAME",
                to: "ansariianas78@gmail.com"
        }
        success {
            mail subject: "Build Success",
                body: "Build no $env.BUILD_NUMBER Success with ID $env.BUILD_ID on node $env.NODE_NAME",
                to: "ansariianas78@gmail.com"
        }
    }
}