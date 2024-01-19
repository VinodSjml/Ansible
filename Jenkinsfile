pipeline{
    agent any
    environment {                                  // Pipeline Variables : All the stages of the pipeline can use it.
        SSH_CRED = credentials('SSH_CRED')
    }
    // parameters {
    //     string(name: 'component'; defaultValue: 'mongodb')
    //     choice(name: 'env'; choices:['dev', 'prod'])
    //}
    stages {
        stage('lint check'){
            when { branch pattern: "feature/", comparator: "REGEXP"}
            steps{
                
                sh ''' echo lint check is in progress
                echo lint check has been completed
                '''
            }
        }
        stage('dry run'){
            when { branch pattern: "PR-.*", comparator: "REGEXP"}
            steps{
                
                sh ''' ansible-playbook -e component=mongodb -e env=env ansible_user=${SSH_CRED_usr} ansible_password=${SSH_CRED_PSW} roboshop-dry.yml           
                '''
            }
        }
        stage('main branch'){
            when {branch 'main'}
            steps{
                
                sh "echo branch is ${BRANCH_NAME}" 
            }
        }

    }
}