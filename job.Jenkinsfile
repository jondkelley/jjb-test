// Jenkins job deployment
// name: core-deployments-jjb
pipeline {
    agent { label 'ecs' }

    environment {
        JENKINS_CREDENTIAL_ID_SSH = 'b315cb66-b24d-468a-9d0c-95a21b00ecee'    // "jenkins (Deploy ssh key)"
    }

    options {
        ansiColor('xterm')
    }

    parameters {
        string(
                name: 'job_definition',
                description: '<big>Select the template to action on, these are in SCM at <pre>jenkins-scripts/jenkins_job_builder/*.yaml</pre></big>'
                )
        choice(
                name: 'jjb_command',
                choices: ['test', 'update'],
                description: '<big>Action for the job builder. <a href="https://docs.openstack.org/infra/jenkins-job-builder/quick-start.html">Learn more</a><br>Detail:<ul><li><b>test</b> - Print XML output of generated job(s) [No change is made]</li><li><b>update</b> - Create or update job(s) from your definition [Does not delete jobs though]</li></ul></big>'
            )
    }
 
    stages {
        stage('job-builder') {
            steps {
                sshagent (credentials: ["${env.JENKINS_CREDENTIAL_ID_SSH}",]) {
                    ansiColor('xterm') {
                        sh "deploy_scripts/jenkins_job_builder ${params.jjb_command} ${params.job_definition}"
                    }
                }
            }
        }
    }
}

