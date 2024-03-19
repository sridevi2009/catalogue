pipeline {
    agent  {
        node {
            label 'agent'
        }
    }
    environment { 
        packageVersion = ''
    }
    options {
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()
        ansiColor ('xterm') 
    }
    // parameters {
    //     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

    //     text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

    //     booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

    //     choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

    //     password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    // }
    
    stages {
        stage('get the version') {
            steps {
                script {
                  def packageJson = readJSON file: 'package.json'
                  packageVersion = packageJson.version
                  echo "application version: $packageVersion"

                }
            }
        }
        stage('install dependencies') {
            steps {
                sh """
                    npm install
                """
            }
        }
        stage('build') {
            steps {
                sh """
                    ls -la
                    zip -r catalogue.zip ./* ".git" -x "*.zip"
                    ls -ltr
                """
            }
        }
        stage('deploy') {
            steps {
                sh """
                    echo " I will run shell-script here "
                    # sleep 10   
                """
            }
        }
        // stage('params') {
        //     steps {
        //         sh """

        //             echo "Hello ${params.PERSON}"

        //             echo "Biography: ${params.BIOGRAPHY}"

        //             echo "Toggle: ${params.TOGGLE}"

        //             echo "Choice: ${params.CHOICE}"

        //             echo "Password: ${params.PASSWORD}"
        //         """    
                
        //     }
        // }
    } 

    post {
        always {
            echo ' I will always say Hello again '
            deleteDir()
        }
        failure {
            echo ' I will always run when the above pipeline is FAILURE '
        }
        success {
            echo ' I will run always when the above pipeline is SUCCESS '
        }
        aborted {
            echo 'I will run when the pipeline is ABORT'
        }

    }
}