pipeline{
    agent any
    parameters {
        string(name: 'git_url', description: 'url of github', defaultValue: 'https://github.com/vanehru/java-hello-world-with-maven.git')
        choice(name: 'deploy', choices: ['true', 'False'], description: 'deploy the jar')

    }
    tools {
         maven 'maven'
         //jdk 'java'
    }

    stages{
        anyOf {
            expression { env.BRANCH_NAME == 'master' }
            expression { env.BRANCH_NAME == 'jenkins' }
              }
                    
        stage('checkout'){
            steps{
                checkout([
                    
                    $class: 'GitSCM', 
                    branches: [[name: "${env.BRANCH_NAME}"]],
                    userRemoteConfigs: [[credentialsId: 'Githubcred', url: "${params.git_url}"]]
                    ])
            }
        }
        stage('build'){
            allOf {
                expression { params.deploy == "true" }
                }
            steps{
               sh 'mvn package'
            }
        }
    }
}
