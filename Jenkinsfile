node {
  
  stage('Code Pickup') {
    git 'https://github.com/RameshThangamuthu/docker_workflow_plugin_demo.git'
  }
  
  stage('Build') {
      appCompileAndPackageImg = docker.build("mec/application-build:${env.BUILD_TAG}", "--file app/Dockerfile.build app")      
    
    /*  
    appCompileAndPackageImg.withRun {       
          buildContainer->sh "/bin/sh"
      }
*/
    
      
      appCompileAndPackageImg.inside {        
        sh "mvn -f app -B clean package"    
        sh "/bin/sh"
      }
    }
/*
  // We are pushing to a private secure Docker registry in this demo.
  // 'docker-registry-login' is the username/password credentials ID as defined in Jenkins Credentials.
  // This is used to authenticate the Docker client to the registry.
  docker.withRegistry('http://localhost/', 'docker-registry-login') {

    def pcImg
    stage('Bake Docker image') {
      // Use the spring-petclinic Dockerfile (see above 'maven.inside()' block)
      // to build a container that can run the app.
      // The Dockerfile is in the app subdir of the active workspace
      // (see above maven.inside() block), so we specify that.
      // The Dockerfile expects the petclinic.war file to be in the 'target' dir
      // relative to its own directory, which will be the case.
      pcImg = docker.build("mec/application:${env.BUILD_TAG}", 'app')

      // Let us tag and push the newly built image. Will tag using the image name provided
      // in the 'docker.build' call above (which included the build number on the tag).
      pcImg.push();
    }

    stage('Test Image') {
      // Spin up a Maven + Xvnc test container, linking it to the petclinic app container
      // allowing the Maven tests to send HTTP requests between the containers.
      def testImg = docker.build('mec/application-tests:snapshot', 'test')
      // Run the petclinic app in its own Docker container.
      pcImg.withRun {petclinic ->
        testImg.inside("--link=${petclinic.id}:petclinic") {
          // https://github.com/jenkinsci/workflow-plugin/blob/master/basic-steps/CORE-STEPS.md#build-wrappers
          wrap([$class: 'Xvnc', takeScreenshot: true, useXauthority: true]) {
            //Ramesh - Offline mode did not work
            //sh "mvn -o -Dmaven.repo.local=${pwd tmp: true}/m2repo -f test -B clean test"
            //sh "Xvnc :10"
            //sh "export DISPLAY=:10"
            //sh "firefox --version"
            sh "mvn -f test -B clean test"
          }
        }
      }
      //input "How do you like ${env.BUILD_URL}artifact/screenshot.jpg?"
      
      /////RAMESH
      //Push the image to a temporary registry and then
      //CREATE an request in BugZilla to approve the build
      ////RAMESH
    }

    /////RAMESH
    //Get this stage invoked only on approval of the Bugzilla ticket. It basically means it will be a seperate job.
    //On approval, invoke the build on the job which takes the local repo and pushes to remote repo
    ///RAMESH
    stage('Promote Image') {
      // All the tests passed. We can now retag and push the 'latest' image.
      //pcImg.push('latest')
    }
  } */
}
