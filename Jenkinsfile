pipeline {
  agent any
  stages {
    stage('stage1') {
      steps {
        sh '''#!/bin/bash
echo "********.Starting CI CD pipeline task.****************"
#-BUILD
echo "........Build phase Started :: Compling source code.........."
cd java_web_code
mvn install

#-Build (TEST)
echo ""
echo  ".....Test Phase Started :: Testing via Automated Scripts :: ........"
cd ../integration-testing/
mvn clean verify -P integration-test'''
      }
    }
    stage('stage2') {
      steps {
        sh '''#!/bin/bash
#-POSTBUILD (PROVISIONING DEPLOYMENT)
echo ""
echo "........Integration phase Started :: Copying Artifacts :: ........."
pwd
echo "pwd in above line"
cd java_web_code/
/bin/cp /var/lib/jenkins/workspace/job1/target/wildfly-spring-boot-sample-1.0.0.war ../docker/
echo ""
echo  ".....Provisioning Phase Started :: Bulding Doker Container :: ........"
cd ../docker/
echo \'eazy123\'|sudo -S docker build -t devops_pipeline_demo .

'''
      }
    }
    stage('stage3') {
      steps {
        sh '''#!/bin/bash

RUNNING=$(echo \'eazy123\'|sudo -S docker inspect -- format="{{ .State.Running }}" $CONTAINER 2> /dev/null)

if [ $? -eq 1 ]
then
  echo "\'$CONTAINER\' DOES NOT EXIST."
else
  echo "eazy123"|sudo -S docker rm -f $CONTAINER
fi

  #-run your container\'
  echo ""
  echo "........Deployment phase Started :: Bulding Docker Container.........."
  echo "eazy123"|sudo -S docker run -d -p 8180:8080 --name devops_pipeline_demo devops_pipeline_demo

#-Completition
echo "--------------------------------------------------------"
echo  "View App deployed her : http://localhost:8180/sample.txt"
echo "--------------------------------------------------------"'''
      }
    }
  }
}