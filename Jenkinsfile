pipeline {
    agent any
    
    environment {                                  // Pipeline Variables : All the stages of the pipeline can use it.
        SSH_CRED = credentials('SSH_CRED')
    }

    // parameters {
    //     string(name: 'COMPONENT', defaultValue: 'mongodb', description: 'Enter the component name') 
    //     choice(name: 'CHOICE', choices: ['dev', 'prod'], description: 'Chose the environment')
    // }

    stages {

        stage('Lint Checks') {                     // This stage will only be executed when you run the job from a feature branch
            when { branch pattern: "feature/.*", comparator: "REGEXP"}
            steps {
                sh '''
                    env 
                    echo **** Starting Lint Checks ****
                    echo **** Lint Checks Completed ****
                '''
            }
        }

        stage('Performing a dry run') {                     // This stage should only run when you raise a PULL Request.
            when { branch pattern: "PR-.*", comparator: "REGEXP"}
            steps {
                sh '''
                    env 
                    ansible-playbook robo-dryrun.yml  -e ENV=dev -e COMPONENT=redis -e ansible_user=${SSH_CRED_USR} -e ansible_password=${SSH_CRED_PSW} 
                '''
            }
        }

        stage('Main Branch') {                     
            when { branch 'main' }
            steps {
                sh '''
                        env
                        echo Name of the branch job running against is ${BRANCH_NAME}
                '''
            }
        }
    }
}