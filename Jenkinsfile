import groovy.io.FileType.*;
node_os  = "";
projectdir = "/var/lib/jenkins/Workspace/${env.BuildName}"
_scala_target_path = "${projectdir}/target"
_artifactpath = "/tmp/artifact"


node {
	try{
		def node_os = isUnix() ? 'LINUX' : 'WIN'
        if (node_os == 'WIN')
            println 'Running on WINDOWS node...!!!'
        else
            println 'Running on LINUX node...!!!'
		ScalaBuild();
		ExecuteCmd();
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

def ExecuteCmd()
{
    sh "mkdir $_artifactpath"
}
def gitPush()
{
 dir(_artifactpath)
 {
 clonerepo(node_os, "github.com/balajiommudali1/scala_artifact.git", "master");
 sh "cp _scala_target_path/*.jar ${_artifactpath}/scala_artifact/"
 deployment();
 //zip dir: _solutionFilePath, glob: "${_buildFileName}", zipFile: _artifactExeFile; 
 sh "git add ."
 sh "git commit -m "test""
 sh "git push"
 }
} 
 def deployment()
 {
 println "Deployment"
 }

def clonerepo(node_os, repo_url, branch){
withCredentials([string(credentialsId: 'comm_git_clone_token', variable: 'comm_git_clone_token')]) {
		if(node_os == 'WIN') {
			bat "git clone -b ${branch} https://${comm_git_clone_token}@${repo_url}";
			println "Cloned the repo";
		}
		else
		{
			sh "git clone -b ${branch} https://${comm_git_clone_token}@${repo_url}"
			println "Cloned the repo";
		}
    }
}
