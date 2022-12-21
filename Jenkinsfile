

pipeline {
    agent {
     label ("node1 || node2 ||  node3 || node4 ||  node5 ||  branch ||  main ||  jenkins-node || docker-agent ||  jenkins-docker2 ||  preproduction ||  production")
            }

options {
    buildDiscarder(logRotator(numToKeepStr: '20'))
    disableConcurrentBuilds()
    timeout (time: 60, unit: 'MINUTES')
    timestamps()
  }

    stages {
        stage('Setup parameters') {
            steps {
                script {
                    properties([
                        parameters([    
                        
                        choice(
                            choices: ['Dev', 'Sanbox','Prod'], 
                            name: 'Environment'   
                                ),

                          string(
                            defaultValue: 's4user',
                            name: 'User',
			                description: 'Required to enter your name',
                            trim: true
                            ),

                          string(
                            defaultValue: 'eric-001',
                            name: 'DB-Tag',
			                description: 'Required to enter the image tag',
                            trim: true
                            ),

                          string(
                            defaultValue: 'eric-001',
                            name: 'UI-Tag',
			                description: 'Required to enter the image tag',
                            trim: true
                            ),

                          string(
                            defaultValue: 'eric-001',
                            name: 'WEATHER-Tag',
			                description: 'Required to enter the image tag',
                            trim: true
                            ),

                          string(
                            defaultValue: 'eric-001',
                            name: 'AUTH-Tag',
			                description: 'Required to enter the image tag',
                            trim: true
                            ),
                        ])
                    ])
                }
            }
        }
 
        
		stage('permission') {
            steps {
                sh '''
cat <<EOF > check.sh
#! /bin/bash 
USER=${User}
cat permission.txt | grep -i $USER
if 
[[ $? -eq 0 ]]
then 
echo "You have permission to run this job"
else 
echo "You DON'T have permission to run this job"
exit 1
fi 
EOF
bash check.sh
                '''
            }
        }
                ls 
		ls
                pwd
		uname -r
                '''
            }
        }
    }
	
	post {
    
    success {
      slackSend (channel: '#development-alerts', color: 'good', message: "Images  have been pushed to Nexus")
    }

    failure {
      slackSend (channel: '#development-alerts', color: '#FF0000', message: "FAILURE: Images  have NOT been pushed to Nexus")
    }
}
}
