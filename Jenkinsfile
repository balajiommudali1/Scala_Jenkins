import groovy.io.FileType.*;
node_os  = "";
projectdir = "/var/lib/jenkins/workspace/${env.Job_Name}"
_scala_target_path = "${projectdir}/target/scala-2.10";
_artifactpath = "/tmp/artifact";
_scala_artifact_path = "${_artifactpath}/scala_artifact"


node {
	try{
		def node_os = isUnix() ? 'LINUX' : 'WIN'
        if (node_os == 'WIN')
            println 'Running on WINDOWS node...!!!'
        else
            println 'Running on LINUX node...!!!'
		ScalaBuild();
		executeCommand();
		archive();
    }catch(Exception e){
		throw e;
	}finally{
	  deleteDir();
	}
}

def ScalaBuild()
{
 clonerepo(node_os, "github.com/balajiommudali1/hello.git", "master");
 echo "Compiling..."
 sh "sbt clean compile package" 
 sh "pwd"
}

def executeCommand()
{
   /* try 
    {
        if(node_os == _windows)
            bat "${cmdStr}";
        else*/
            sh "if [ ! -d ${_artifactpath} ]; then mkdir ${_artifactpath}; fi";
  /*  }
    catch(Exception e) 
    {
        println "${_unicodeError} Error occurred while executing command";
        throw e;
    }*/
}
def archive()
{
 dir(_artifactpath)
 {
 deleteDir();
 
 clonerepo(node_os, "github.com/balajiommudali1/scala_artifact.git", "master");
 sh "cp ${_scala_target_path}/*.jar ${_scala_artifact_path}"
 sh "pwd"
 GitPush();
 Deployment();
 //zip dir: _solutionFilePath, glob: "${_buildFileName}", zipFile: _artifactExeFile; 
 } 
} 

 def GitPush()
 {
 dir(_scala_artifact_path)
 {
 withCredentials([string(credentialsId: 'comm_git_clone_token1', variable: 'comm_git_clone_token1')]) {
 println "Git push"
 sh "git add ."
 sh "git commit -m 'test' "
 sh "git remote set-url origin https://${comm_git_clone_token1}@github.com/balajiommudali1/scala_artifact.git"
 sh "git push origin master"
 }
 }
 }
 def Deployment()
 {
 // withCredentials(bindings: [sshUserPrivateKey(credentialsId: 'appnode', keyFileVariable: 'appnode', passphraseVariable: '', usernameVariable: 'ubuntu')]) {
  sh " scp -i /var/lib/jenkins/appnode.pem ${_scala_target_path}/*.jar ubuntu@3.83.164.48:/tmp"
  sh " ssh -i /var/lib/jenkins/appnode.pem ubuntu@3.83.164.48 sudo java -jar /tmp/scala_*.jar"
//}
 } 

def clonerepo(node_os, repo_url, branch){
withCredentials([string(credentialsId: 'comm_git_clone_token1', variable: 'comm_git_clone_token1')]) {
		if(node_os == 'WIN') {
			bat "git clone -b ${branch} https://${comm_git_clone_token1}@${repo_url}";
			println "Cloned the repo";
		}
		else
		{
			sh "git clone -b ${branch} https://${comm_git_clone_token1}@${repo_url}"
			println "Cloned the repo";
		}
    }
}
