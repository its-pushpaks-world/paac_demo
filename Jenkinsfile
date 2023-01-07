pipeline {
    agent any
	triggers {
        githubPush()
      }
    options { timestamps () }
	parameters {
			booleanParam name:"DRY_RUN", description:"dryrun?", defaultValue:true
		        choice name:'ACTIVITY', description:"What should be done?", choices:['Backup','Check_config', 'Copy', 'Deployment', 'Test_connectivity']
    			string name:'DELIVERY_ID', description:"D2CCM Delivery ID", defaultValue:"KIAS2000_SN_0000"
	      		string name:'TAR_FILE', description:"Name of tar file without ending (Entry can also be empty)(.zip, .tar.gz and tar supported)"
			choice name:'APPLICATION', description:"Where should software be deployed?", choices:['bicc','cs']
			choice name:'SUBSYSTEM', description:"subsystem?", choices:['laura1','laura2']
			choice name:'DEPLOY_ENVIRONMENT', description:"Temporary Variable", choices:['PRD','DEV']
			booleanParam name:"TESTMODE", description:"TESTMODE?", defaultValue:false
			booleanParam name:'DEBUG', description:"Additional debug output?", defaultValue:true
			booleanParam name:'E_MAIL', description:"Create E_MAIL?", defaultValue:false
			string name:'E_MAIL_ADDRESS', description:"Recipient(s) of job status/result (comma or semicolon separated)", defaultValue:"itspushpaksworld496@gmail.com;"
			string name:'TICKET', description:"Ticket number for SOX update (only for PRD environment)", defaultValue:"Z_SRT0000A"
			}
    stages {
        
        stage('Get Input') {
		
		/* input {
                	message "Can we Proceed?"
                	ok "Yes"
                	submitter "project1, pushpak2"
		} */
            steps {
                sh 'echo "Get Input" '
            }
        }
        
        stage('Test Connectivity') {
            steps {
                sh 'echo "Test Connectivity" '
            }
        }
        
        stage('Check config') {
            steps {
                sh 'echo "Check config" '
            }
        }
        
       
        
        stage('Copy from D2CCM') {
            steps {
                //Copy data from source
                sh 'cp -f /home/pushpak/Delivery/${DELIVERY_ID}/${TAR_FILE} . '
            }
        }
        
        stage('Backup') {
            steps {
                sh 'echo "Backup" '
            }
        }
        
        stage('Deploy') {
            steps {
                //Untarring compressed file //start of sh
                //sh '''
		
		sh ' pwd'
		sh ' tar -xvf ${TAR_FILE}'
		
		//Create log file
               sh ' ls -lrt'
	       sh 'cur_time=`date +"%m_%d_%Y_%H_%M_%S"`'
		    sh ' echo ${cur_time} '
		    sh ' cat >> ${DELIVERY_ID}.log.txt'
                
                //Add Run time in log
                sh ' date >> ${DELIVERY_ID}.log.txt'
                
                //Remove tar and adding in log file
               sh ' echo "Removing ${TAR_FILE}" >> ${DELIVERY_ID}.log.txt'
                sh ' rm ${TAR_FILE}'
                sh ' ls -lrt'
		    //''' 
		    //End of sh
            }
        }
        
    }
	post {
		success { 
			script{
			
			if (params.E_MAIL)
			{
			
			emailext to: "${E_MAIL_ADDRESS}",
			subject: "generic_delivery: Deployment of ${DELIVERY_ID} in ${DEPLOY_ENVIRONMENT} (${APPLICATION}) finished with RESULT: ${currentBuild.currentResult}",
           		body: ": Job ${env.JOB_NAME} \n More Info can be found here: ${env.BUILD_URL} \n DRY_RUN:${DRY_RUN} \n ACTIVITY:${ACTIVITY}\n DELIVERY_ID:${DELIVERY_ID} \n TAR_FILE:${TAR_FILE} \n APPLICATION:${APPLICATION} \n SUBSYSTEM:${SUBSYSTEM} \n DEPLOY_ENVIRONMENT:${DEPLOY_ENVIRONMENT} \n TESTMODE:${TESTMODE} \n DEBUG:${DEBUG} \n E_MAIL:${E_MAIL} \n E_MAIL_ADDRESS:${E_MAIL_ADDRESS} \n TICKET:${TICKET}"
			attachlog: true
			attachmentsPattern: '${DELIVERY_ID}.log.txt'

			}  //end of if
				
			if (!params.E_MAIL){ echo "Mail not sent as \${E_MAIL} is unset"}
				
			}
			
		}
	}
}

