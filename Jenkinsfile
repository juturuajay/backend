pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time:10, unit: "MINUTES")
        disableConcurrentBuilds()
        //retry(1)
    }

    environment{
        DEBUG = 'true'
        appVersion = '' //this will become global
    }

    stages {
        stage('Read the version') {
            steps {
             script{
               def packageJson = readJSON file: 'package.json'
               appVersion = packageJson.version
               echo " App Version : ${appVersion} "
             } 
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Deploy') {
            when {
                expression { env.GIT_BRANCH == "origin/main" }
            }
            steps {
                echo 'Deploying'
                //error "pipeline failure"
            }
        }

        stage('Approval') {
            input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps {
                echo "Hello, ${PERSON}, nice to meet you."
            }
        }
    }
    post {
        always{
            echo "this section run always"
            deleteDir()
        }
        success{
            echo "this section runs when pipeline success"
        }
        failure{
            echo "this section run when pipeline failure"
        }
    }
}