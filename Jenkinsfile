pipeline{
    agent{
        label 'slave-1'
    }

    tools{
      jdk  'JDK-17'
      maven 'maven-8.8'
    }

    environment{
      Application_Name = "eureka"
      SONAR_TOKEN = credentials('sonar_creds')
      SONAR_URL = "http://34.29.155.140:9000"
      POM_VERSION = readMavenPom().getVersion() // readMavenPom() reads the pom the xml file, getVersion() get version will display version from reading the pom.xma and stores in "POM_VERSION" 
      POM_PACKAGING = readMavenPom().getPackaging() // it will display 'jar','var' file and stores in "POM_Pacakaging"
      DOKER_HUB = "docker.io/rakesh9182"  // use this detail to pull or push the docker images, "rakesh9182" is your user name
      DOCKER_CREDS = credentials('docker_creds')
    }
  stages{
    stage('buildstage'){
        steps{
            echo "building the ${env.Application_Name} Application"
            sh 'mvn clean package -DskipTests=true'
        }
    }

    stage('sonarstage'){
      steps{
        echo "starting sonar scan"
        //  "withSonarQubeEnv" 
        withSonarQubeEnv('sonar'){  // This name is you saved under manage jenkins in sonar "sonar"
            sh """
            mvn sonar:sonar \
                -Dsonar.projectKey=i27-eureka \
                -Dsonar.host.url="${env.SONAR_URL}" \
                -Dsonar.login=${SONAR_TOKEN}
            """
          
        }
        timeout (time: 2, unit: 'MINUTES'){ // it will wait for 2 minutes
            waitForQualityGate abortPipeline: true //This line helps if build success go to next step if build fails then stop the build
        }
      }
    }

    stage('Displaying POM name'){
      steps{
        // for this we need to install "pipeline-utility-steps" plugin
        // it will just printnot change the file name
        //My Articate file name looks like : i27-eureka-0.0.1-SNAPSHOT.Jar
        echo "My Jar Sorce like: i27-${env.Application_Name}-${env.POM_VERSION}.${env.POM_PACKAGING}"
        // I want to see my rtifact name like this: i27-eureka-20-main.jar
        echo "Required display name like this: i27-${env.Application_Name}-${BUILD_NUMBER}-${BRANCH_NAME}.${env.POM_PACKAGING}"
      }
    } 

    stage('Docker Build'){
      steps{
        // using shell to write the multiple command
        // "pwd" display the current working directory to see and setup the dockerfile path if the dockerfile is in different location then it will fail
        // "ls -la" available files in that directory, to cross check if "Dockerfile" is there are not
        // "GIT_COMMIT" we use this as a tag becuase tag name keeps on changing, so GIT_COMMIT id also changes so we use this as a tag
        // "/.cicd" under this folder my dockerfile present
        // "--build-arg JAR_SOURCE=i27-${env.Application_Name}-${env.POM_VERSION}.${env.POM_PACKAGING}" Passing the "JAR_SOURCE" environmental variables dynamically
        // becuase we write in dockerfile "JAR_SOURCE" argument is required for the dockerfile
        // "i27-${env.Application_Name}-${env.POM_VERSION}.${env.POM_PACKAGING}"  this is the jar file if you see in your dockerfile you write the copy command to copy this file to your required location
        sh """
           echo "*** Building the docker ***"
           pwd
           ls -la
           # copying the jar to the ".cicd" 
           # Workspace means pickup the curent working
           cp ${WORKSPACE}/target/i27-${env.Application_Name}-${env.POM_VERSION}.${env.POM_PACKAGING} ./.cicd
           ls -la ./.cicd
           # syntax: docker build -t imagename:tag dockerfilepath
           docker build --no-cache --build-arg JAR_SOURCE=i27-${env.Application_Name}-${env.POM_VERSION}.${env.POM_PACKAGING} -t ${env.DOCKER_HUB}/${env.Application_Name}:${GIT_COMMIT} ./.cicd
           # above line like this: docker build -t docker.io/rakesh9182/eureka:gitcommitid
        """
      }
    }
    stage('Docker Push'){
      steps{
        echo " ***** Pushing image to Docker Registry *****"
        // "DOCKER_CREDS_USR" takes the user name, "DOCKER_CREDS_PSW" takes the password
        // It will push to "docker.io/rakesh9182/eureka:122bcs"
        sh """
           
           docker login -u ${DOCKER_CREDS_USR} -p ${DOCKER_CREDS_PSW}
           docker push ${env.DOCKER_HUB}/${env.Application_Name}:${GIT_COMMIT}
           
        """
      }
    }
  }

}
