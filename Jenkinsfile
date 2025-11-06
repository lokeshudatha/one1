pipeline{

    agent any
    
    environment{
        APP = "myapp"
    }

    parameters{
        string(name: 'USER', defaultValue: 'Lokesh_Udatha', description: 'Enter your name here: ')
        booleanParam(name: 'Test', defaultValue: true, description: 'Running the test tasks here.')
        choice(name: 'Env', defaultValue: ['Build', 'Test', 'Prod'], description: 'Select one env here: ')
    }
    stages{
        stage('Input for user'){
            environment{
                A = "HR-462"
            }
            when{
                expression{
                    return params.Env == 'Build'
                }
            }
            steps{
                input message: 'Do you want to processed next ....?', ok: 'Yes, Continue.', submitter: 'admin'
                echo "Finally, Input block is complated"
            }
        }
        parallel{
            stage('Build'){
                when{
                    allOf{
                        expression{
                            return params.Env == 'Build'
                        }
                        expression{
                            return params.Test == true 
                        }
                    }
                }
                
                steps{
                    echo "I'm parallel block. I'll run some commands below..."
                    script{
                        sh 'mkdir lokesh'
                        sh 'cd lokesh'
                    }
                }
            }
            stage('test'){
                parallel{
                    stage('one'){
                        steps{
                            echo "sub-one-stage"
                        }
                    }
                    stage('two'){
                        parallel{
                            stage('multi-stage'){
                                steps{
                                    echo "This is my final stage of pipeline"
                                }
                            }
                            stage('multi-two-stage'){
                                steps{
                                    echo "THE END"
                                }
                            }
                        }
                        steps{
                            echo "sub-two-stage"
                        }
                    }
                }
                when{
                    expression{
                        return env.A == "HR-462"
                    }
                    steps{
                        script{
                            try{
                                echo "If it is correct. ${params.Env}"
                                sh 'git --version'
                            } catch(Exception e){
                                error("stopping the pipeline because of someerrors in my pipeline ${e.message}")
                            } finally{
                                echo "pipeline is complated."
                            }
                        }
                    }
                }
            }
        }
        parallel{
            stage('final-touch'){
                steps{
                    script{
                        sh 'mkdir final'
                        sh 'cd final'
                        sh 'touch fileone'
                    }
                }
            }
            stage('semi-final'){
                steps{
                    script{
                        sh 'mkdir semi'
                        sh 'cd semi'
                        sh 'touch two'
                        sh 'echo "Hello, Some data in filetwo" >> two'
                    }
                }
            }
        }
    }
    post{
        success{
            echo "Pipeline is complated successfully"
        } 
        failure{
            echo "My pipeline is fail, So ${message}"
        }
        always{
            echo "Feel free to ask everything."
        }
    }
}
