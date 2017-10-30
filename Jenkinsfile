#!/usr/bin/env groovy

def app=""
def env=""
def cmd=""


node {
    
    stage('Clone sources') {
        git url: 'https://github.com/OpenMake-Software/DeployHub-Pro-Pipeline.git'
    }
    
    stage ('Testing') {
      echo "**********************************************************************************"    
      echo "* Moving $app from Integration to Testing"
      echo "**********************************************************************************"
      cmd = /dhmove.py --app ${app} --from_domain 'GLOBAL.My Pipeline.Integration' --task 'Move to Testing'/
      sh cmd        
        
      echo "**********************************************************************************"        
      echo "* Deploying $app to Testing"
      echo "**********************************************************************************"
      cmd = /dhdeploy.py --app ${app} --env 'Uptime Testing'/
      sh cmd

      echo "**********************************************************************************"    
      echo "* Running Testcases for $app in Testing"
      echo "**********************************************************************************"    
      cmd = /runtestcases.py --app ${app} --env Testing/
      sh cmd
    }
    
    stage ('Production') {
      echo "**********************************************************************************"    
      echo "* Moving $app from Testing to Production"
      echo "**********************************************************************************"    
      cmd = /dhmove.py --app ${app} --from_domain 'GLOBAL.My Pipeline.Testing' --task 'Move to Production'/
      sh cmd     

      echo "**********************************************************************************"        
      echo "* Deploying $app to Production"
      echo "**********************************************************************************"
      cmd = /dhdeploy.py --app ${app} --env 'Uptime Production'/
      sh cmd  
    }
}
