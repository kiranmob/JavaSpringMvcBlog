node 
{ 
 stage ('SCM Checkout') 
{ 
 git 'https://github.com/kiranmob/JavaSpringMvcBlog.git' 
} 
 stage ('Test Package') 
{ 
 sh 'mvn test' 
} 
// stage ('Sonarqube ') 
//{ 
//  withSonarQubeEnv('Sonarcloud') { 
//   sh "mvn sonar:sonar" 
//  } 
// } 
//   stage("Quality Gate Check") 
// { 
//  timeout(time: 1, unit: 'HOURS') 
// { 
//   def qg = waitForQualityGate() 
//   if (qg.status != 'OK') 
//{ 
//  error "Pipeline aborted due to quality gate failure: ${qg.status}" 
//    } 
//   } 
// } 
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
  def server = Artifactory.server 'jenkins_artifactory' 
  def uploadSpec = """{ 
  "files": [ 
    { 
      "pattern": "/var/jenkins_home/workspace/Maven-Job/maven-cicd-pipleine/target/*.war",
      "target": "blog-snapshot" 
 } 
  ] 
    }""" 
  server.upload(uploadSpec)
  } 
 stage('Downloading artifact') 
 { 
  def server = Artifactory.server 'jenkins_artifactory' 
  def downloadSpec="""{ 
  "files":[ 
  { 18 

   "pattern":"blog-snapshot/blog.war", 
   "target":"/var/jenkins_home/war/" 
   } 
  ] 
  }"""  
  server.download(downloadSpec) 
} 
stage('Deploy to Tomcat') 
{ 
    sh 'scp /var/jenkins_home/war/blog.war ubuntu@54.90.157.179:/usr/local/apache-tomcat-8.5.63/webapps/' 
  } 
 }
