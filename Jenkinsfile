pipeline {
    agent any

    environment {
        APP = "myapp"
    }

    parameters {
        string(name: 'USER', defaultValue: 'Lokesh_Udatha', description: 'Enter your name here:')
        booleanParam(name: 'Test', defaultValue: true, description: 'Run test tasks here.')
        choice(name: 'Env', choices: ['Build', 'Test', 'Prod'], description: 'Select environment:')
    }

    stages {

        stage('Input for user') {
            environment {
                A = "HR-462"
            }
            when {
                expression { return params.Env == 'Build' }
            }
            steps {
                input message: 'Do you want to proceed next?', ok: 'Yes, Continue.', submitter: 'admin'
                echo "Input block completed."
            }
        }

        stage('Build & Test in Parallel') {
            parallel {

                stage('Build') {
                    when {
                        allOf {
                            expression { return params.Env == 'Build' }
                            expression { return params.Test == true }
                        }
                    }
                    steps {
                        echo "Build stage running..."
                        sh '''
                            mkdir -p lokesh
                            cd lokesh
                            echo "Inside build directory"
                        '''
                    }
                }

                stage('Test') {
                    when {
                        expression { return env.A == "HR-462" }
                    }
                    stages {
                        stage('One') {
                            steps {
                                echo "sub-one-stage"
                            }
                        }

                        stage('Two and its sub-stages') {
                            steps {
                                echo "sub-two-stage"
                            }
                        }

                        stage('Multi One') {
                            steps {
                                echo "This is my final stage of pipeline"
                            }
                        }

                        stage('Multi Two') {
                            steps {
                                echo "THE END"
                            }
                        }
                    }
                }
            }
        }

        stage('Final Parallel') {
            parallel {
                stage('final-touch') {
                    steps {
                        sh '''
                            mkdir -p final
                            cd final
                            touch fileone
                        '''
                    }
                }

                stage('semi-final') {
                    steps {
                        sh '''
                            mkdir -p semi
                            cd semi
                            touch two
                            echo "Hello, some data in filetwo" >> two
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed."
        }
        always {
            echo "Feel free to ask everything."
        }
    }
}
