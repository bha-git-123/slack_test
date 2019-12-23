#!/usr/bin/env groovy
podTemplate (containers: [
        containerTemplate(name: 'oc-4-1', image:'quay.io/openshift/origin-jenkins-agent-base:4.3', ttyEnabled: true, command: 'cat'),
       ]){
    
    
    node(POD_LABEL) {  
    
    currentBuild.result = "SUCCESS"
         try {

                  stage('Checkout'){

                       checkout scm
                      /* git branch: 'master',credentialsId: '',url: '' */
                          
                  }
                  
            
                  stage('Generate Report') {           
                      container('oc-4-1') {
			      
			      
			      
			      sh script:'oc login https://c100-e.us-south.containers.cloud.ibm.com:32044 --token=NFGtewL305Xcj_in_mRS1dsOaq7VweiivjvrSknvXJ8'
			      
                              def output = sh returnStdout: true, script: 'sh main-script2.sh 2>&1 | tee OCP_Status_Report.txt'
                              // println output  
                              // def op = sh returnStdout: true, script: 'OCP_Status_Report.txt'
			      // println op
			      slackSend (channel: '#automation', message: output , color: 'good', failOnError: true, teamDomain: 'gtsmcms')
			      slackUploadFile channel: "#automation", filePath: "OCP_Status_Report.txt", credentialId: "1221906b-56a4-4331-a811-2404ed5fc31e" ,initialComment:  'Complete Report'

			      // sleep(time:20,unit:"SECONDS")
			      
			      //slackUploadFile doesnot work with thread use SLACK API for upload 
                              //sh script: 'curl -F file="@script-output.txt" -F channels="CPME1B70C"  -H "Authorization: Bearer <update bot token here>" https://slack.com/api/files.upload;'
                               
			      
                              /* def blocks = [
                                   [
	                 	"type": "section",
		                "text": [
			         "type": "mrkdwn",
			         "text": output
                                        ]
	                            ]
                                 ] */
                              //slackSend(channel: '#mcms-iscp-devop-testing', blocks: blocks , color: '#3eb991', failOnError: true, teamDomain: 'cicindia-ibmgts', token: 'qbfUngJSJYimWJ15jeHVfI0w')
      
                      }
        
                      
                     
           
                      
                  }
     
               

    }
    
    catch (err) {

        currentBuild.result = "FAILURE"

            
        throw err
    }

      
}
}    
