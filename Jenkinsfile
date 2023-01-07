pipeline {
    agent any
    options { timestamps () }
	parameters {
			booleanParam name:"DRY_RUN", description:"dryrun?", defaultValue:true
      string name:'temp_var', description:"Temporary Variable", defaultValue:"Temporary"
      choice name:'ACTIVITY', description:"What should be done?", choices:['Backup','Check_config', 'Copy', 'Deployment', 'Test_connectivity']
      string name:'DELIVERY_ID', description:"D2CCM Delivery ID", defaultValue:"KIAS2000_SN_0000"
      string name:'TAR_FILE', description:"Name of tar file without ending (Entry can also be empty)(.zip, .tar.gz and tar supported)"
      choice name:'APPLICATION', description:"Where should software be deployed?", choices:['bicc','cs']
      choice name:'DEPLOY_ENVIRONMENT', description:"Temporary Variable", choices:['prod','dev']
      booleanParam name:"DRY_RUN", description:"dryrun?", defaultValue:true
      booleanParam name:"TESTMODE", description:"TESTMODE?", defaultValue:false
      booleanParam name:'DEBUG', description:"Additional debug output?", defaultValue:false
      booleanParam name:'E_MAIL', description:"Create E_MAIL?", defaultValue:false
      string name:'E_MAIL_ADDRESS', description:"itspushpaksworld496@gmail.com;", defaultValue:"Recipient(s) of job status/result (comma or semicolon separated)"
      string name:'TICKET', description:"Ticket number for SOX update (only for PRD environment)", defaultValue:"Z_SRT0000A"
			}
    stages {
        
        stage('Get Input') {
            steps {
                sh 'echo "Get Input" '
            }
        }
        
        /*stage('Test Connectivity') {
            steps {
                sh 'echo "Test Connectivity" '
            }
        }*/
        
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
                //Untarring compressed file
                sh 'tar -xvf ${TAR_FILE}'
            }
        }
        stage('post') {
            steps {
                //Create log file
                sh 'ls -lrt'
                sh 'touch ${DELIVERY_ID}.log.txt'
                
                //Add Run time in log'
                sh 'date >> ${DELIVERY_ID}.log.txt'
                
                //Remove tar and adding in log file
                sh 'echo "Removing ${TAR_FILE}" >> ${DELIVERY_ID}.log.txt'
                sh 'rm ${TAR_FILE}'
                sh 'ls -lrt'
            }
        }
    }
}
