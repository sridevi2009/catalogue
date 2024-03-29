pipeline {
    agent  {
        node {
            label 'agent'
        }
    }
    environment { 
        packageVersion = ''
        nexusURL = '172.31.2.241:8081'
    }
    options {
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()
        ansiColor ('xterm') 
    }
     parameters {
    //     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

    //     text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'Deploy', defaultValue: false, description: 'Toggle this value')

    //     choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

    //     password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
     }
    
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
        stage('unit tests') {
            steps {
                sh """
                    echo "unit tests will run here" 
                """
            }
        }
        stage('sonar scan') {
            steps {
                sh """
                    sonar-scanner
                """
            }
        }
        stage('build') {
            steps {
                sh """
                    ls -la
                    zip -q -r catalogue.zip ./* ".git" -x "*.zip"
                    ls -ltr
                """
            }

        }
        stage('publish Artifact') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexusURL}",
                    groupId: 'com.roboshop',
                    version: "${packageVersion}",
                    repository: 'catalogue',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                        [artifactId: 'catalogue',
                        classifier: '',
                        file: 'catalogue.zip',
                        type: 'zip']
                    ]
                )
            }
        }
        stage('Deploy') {
            when {
                expression {
                    params.Deploy == 'true'
                }    
            }
            steps {
                script {
                        def params = [
                            string(name: 'version', value: "$packageVersion"),
                            string(name: 'environment', value: "dev")
                        ]
                        build job: "catalogue-deploy", wait: true, parameters: params    
                    }    
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