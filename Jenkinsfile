pipeline {

  agent any

  stages {	
	  

	stage('Maven Compile'){

		steps{

			echo 'Project compile stage'

			bat label: 'Compilation running', script: '''mvn compile'''

	       	}

	}

	

	stage('Unit Test') {

	   steps {

			echo 'Project Testing stage'

			bat label: 'Test running', script: '''mvn test'''

	       
		   

       }

   	}   

    stage('Jacoco Coverage Report') {

        steps{

            jacoco()

		}

	}

        

	stage('SonarQube'){

         steps{

            bat label: '', script: '''mvn sonar:sonar \

		 -Dsonar.host.url=http://localhost:9000 \

 		-Dsonar.login=e2036db293247cc969d108f1b42e6378c9dd9ec5'''

          }

	}
	  
        
	   stage('Jmeter'){
         steps{
	    // cd 	 C:\Program Files\apache-jmeter-5.3\bin
            bat label: 'jmeter',script:'C:\\apache-jmeter-5.3\\bin\\jmeter -n -Jjmeter.save.saveservice.output_format=xml -t C:\\Users\\natreddy\\Documents\\folder_for_jmeter\\SpringBootDataJpaApp -l C:\\Users\\natreddy\\Documents\\folder_for_jmeter\\Test-emp.jtl'
         
	 }
	}
	  stage('output'){
		  steps{
			   perfReport filterRegex: '', sourceDataFiles: 'C:\\Users\\kanram\\Desktop\\POD2\\results\\Test.jtl'
		  }
	  }

	

	stage('Maven Package'){

		steps{

			echo 'Project packaging stage'

			bat label: 'Project packaging', script: '''mvn package'''

		}

	} 	
	  stage('Ok') {
            steps {
                echo "Ok"
            }
        }
    }
    post {
        success {
            emailext body: 'A Test EMail', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test'
        }
	     failure {
        mail to: 'mithunputhusseri@gmail.com',
             subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
             body: "Something is wrong with ${env.BUILD_URL}"
    }
    }
	 
     
   

  

}
