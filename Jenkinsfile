pipeline{
	agent any
	triggers{
		pollSCM('* * * * *')
	}
	stages{
		stage("prepare build"){
			stepes{
				echo "prepare - build"
			}
		}
		stage("run build"){
			stepes{
				echo "run- build"
			}
		}
	}
}
