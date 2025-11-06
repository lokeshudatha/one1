pipeline {
    agent any

    environment {
        APP = "myapp"
    }

    parameters {
        string(name: 'USER', defaultValue: 'Lokesh_Udatha', description: 'Enter your name here:')
        booleanParam(name: 'Test', defaultValue: true, description: 'Running the test tasks here.')
        choice(name: 'Env', choices: ['Build', 'Test', 'Prod'], description: 'Select one env here:')
    }

    stages {

        stage('Input for user') {
            environment {
                A = "HR-462"
            }
            when {
                expression {
                    return params.Env == 'Build'
                }
            }
            steps {
                input message: 'Do you want to proceed next ....?', ok: 'Yes, Continue.', submitter: 'admin'
                echo "Finally, Input block is completed"
            }
        }

        stage('Parallel Tasks') {
            parallel {
                stage('Build') {
                    when {
                        allOf {
                            expression { return params.Env == 'Build' }
                            expression { return params.Test == true }
                        }
                    }
                    steps {
                        echo "I'm in a parallel block. I'll run some commands below..."
                        script {
                            sh '''
                                mkdir -p lokesh
                                cd lokesh
                                echo "Inside lokesh directory"
                            '''
                        }
                    }
                }

                stage('Test') {
                    when {
                        expression { return env.A == "HR-462" }
                    }
                    steps {
                        echo "Running test stages..."
                        script {
                            try {
                                echo "If it is correct. Environment: ${params.Env}"
                                sh 'git --version'
                            } catch (Exception e) {
                                error("Stopping the pipeline because of some errors in my pipeline: ${e.message}")
                            } finally {
                                echo "Pipeline is completed."
                            }
                        }
                    }

                    stages {
                        stage('One') {
                            steps {
                                echo "sub-one-stage"
                            }
                        }
                        stage('Two') {
                            parallel {
                                stage('multi-stage') {
                                    steps {
                                        echo "This is my final stage of pipeline"
                                    }
                                }
                                stage('multi-two-stage') {
                                    steps {
                                        echo "THE END"
                                    }
                                }
                            }
                            steps {
                                echo "sub-two-stage"
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
                        script {
                            sh '''
                                mkdir -p final
                                cd final
                                touch fileone
                            '''
                        }
                    }
                }

                stage('semi-final') {
                    steps {
                        script {
                            sh '''
                                mkdir -p semi
                                cd semi
                                touch two
                                echo "Hello, Some data in filetwo" >> two
                            '''
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully"
        }
        failure {
            echo "My pipeline failed."
        }
        always {
            echo "Feel free to ask everything."
        }
    }
}
