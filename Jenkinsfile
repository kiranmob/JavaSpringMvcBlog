node 
{ 
 stage ('SCM Checkout') 
{ 
 git 'https://github.com/ajit2525/JavaSpringMvcBlog-1.git' 
} 
 stage ('Test Package') 
{ 
 sh 'mvn test' 
} 
 stage ('Sonarqube ') 
{ 
  withSonarQubeEnv('jenkins_sonarqube') { 
   sh "mvn sonar:sonar" 
  } 
 } 
   stage("Quality Gate Check") 
 { 
  timeout(time: 1, unit: 'HOURS') 
 { 
   def qg = waitForQualityGate() 
   if (qg.status != 'OK') 
{ 
  error "Pipeline aborted due to quality gate failure: ${qg.status}" 
    } 
   } 
 } 
 try 
 { 
  stage ('Build Package') 
 { 
   sh 'mvn package' 
  }
 } 
 catch(err) 
  { 
   stage ('Email Notification') 
     { 
        emailext body: '''Hi 
        Your build has failed. Please rectify the error. 
        Regards''', subject: 'Build Failed', to: 'ajiteee2394@gmail.com' 
   } 
 }
 stage ('Artifact upload') 
{ 
  def server = Artifactory.server 'jenkins_artifactory' def uploadSpec = """{ 
  "files": [ 
    { 
      "pattern": "/var/lib/jenkins/workspace/PluralSight1/target/*.war",
      "target": "Blog-snapshot" 
 } 
  ] 
    }""" 
  server.upload(uploadSpec)\ 
  } 
 }
