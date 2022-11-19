pipeline{
    agent any
    parameters {
        string(name: 'git_url', description: 'url of github', defaultValue: 'https://github.com/vanehru/java-hello-world-with-maven.git')
        choice(name: 'deploy', choices: ['true', 'False'], description: 'deploy the jar')

    }
    tools {
         maven 'maven'
    }

    stages{
            stage('clone'){
                when {
                    anyOf {
                        expression { env.BRANCH_NAME == 'master' }
                        expression { env.BRANCH_NAME == 'jenkins' }
                        }
                }
                steps{
                    checkout([
                        $class: 'GitSCM', 
                        branches: [[name: "${env.BRANCH_NAME}"]],
                        userRemoteConfigs: [[credentialsId: 'Githubcred', url: "${params.git_url}"]]
                        ])
                    }
                }
            stage('build'){
                when {
                    allOf {
                        anyOf {
                            expression { env.BRANCH_NAME == 'master' }
                            expression { env.BRANCH_NAME == 'jenkins' }
                            }
                        expression { params.deploy == "true" }
                        }
                    }
                steps{
                sh 'mvn package'
                }
            }
    }
}
