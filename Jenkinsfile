pipeline {
  agent { node { label 'slave01' } }

   stages {
      stage('Clone Sources') {
        steps {
          checkout scm
        } 
      }
	stage('Build') {
         steps {
            echo 'Build process..'            
            sh '''
                cd /home/slave/workspace/finel_project
                chmod 755 *.sh
		chmod 755 *.py
		chmod 755 *.c
		chmod 755 *.java
            '''
         }
      }      
        stage("Env Variables") {
            steps {
                sh "printenv"
            }
        }
	 stage('Bash') 
	   {
		 when{
			 expression { PARAM == 'BASH'}
		 }
		 steps {
			 sh '''
			   echo 'Execute bash script'
		      	   echo "Testing input string $PARAM"
			   cd /home/slave/workspace/finel_project
			   ./bash.sh $PARAM
			 '''
		 }	
          }
	   stage ('PYTHON') 
	   {
      		when {
                expression { PARAM == 'PYTHON'}
            	}
            	steps {
                	sh '''
			  echo "Execute python script"
		      	  echo "Testing input string $PARAM" 
            	      	  cd /home/slave/workspace/finel_project
                      	  python python.py $PARAM
                      	  python python.py $PARAM >> /home/slave/results
			'''
            	}
    	   }
	   stage ('C') 
	   {
      		when {
                expression { PARAM == 'C'}
            	}
            	steps {
                	sh '''
			  echo 'Execute C script'
			  echo "Testing input string $PARAM"
			  cd /home/slave/workspace/finel_project
			  ./Cfile.c $PARAM
			  ./Cfile.c $PARAM >> /home/slave/results
			'''
            	}
    	   }
	   stage ('ALL') 
	   {
      		when {
                expression { PARAM == 'ALL'}
            	}
            	steps {
                	sh '''
			  echo 'Execute ALL script'
			  cd /home/slave/workspace/finel_project
			  python python.py $PARAM
			  python python.py $PARAM >> /home/slave/results
			  ./Cfile.c $PARAM
		          ./Cfile.c $PARAM >> /home/slave/results
			  ./bash.sh $PARAM
		          ./bash.sh $PARAM >> /home/slave/results
			'''
            	}
    	   }
	   stage ('javaFile') 
	   {
      		when {
                expression { PARAM == 'java'}
            	}
            	steps {
                	sh '''
			  echo "Execute javaFile script"
			  cd /home/slave/workspace/finel_project
			  ./javaFile.py $PARAM
			  javaFile.py $PARAM >> /home/slave/results
			'''
            	}
    	   }
	stage('Saving Results') {
         steps {
            echo 'Saving Results process..'
            sh '''
	      report_file="${HOME}/Documents/Deployment/report"
              mkdir -p ${HOME}/Documents/Deployment/              
              if [ -f "${report_file}" ]; then
                echo "file ${report_file} exists"
              else
	              touch ${report_file}
              fi  
	      touch ${WORKSPACE}/scripts
	      touch ${WORKSPACE}/scripts/results
              echo "Build Number $BUILD_NUMBER" >> ${report_file}
	      cat ${WORKSPACE}/scripts/results >> ${report_file}
	      echo "#############################" >> ${report_file}
            '''
         }
      }	   
      
      
   }
}
