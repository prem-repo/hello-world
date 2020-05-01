pipeline{
	agent any
	triggers{
		pollSCM('* * * * *')
	}
	stages{
		stage("prepare build"){
			steps{
				echo "prepare - build"
			}
		}
		stage("run build"){
			steps{
				echo "run - build"
			}
		}
	}
}
