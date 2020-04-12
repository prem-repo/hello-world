node {
   stage('SCM Checkout'){
     git 'https://github.com/prem-repo/jenkins'
   }
   stage('Compile-Package'){
    sh 'mvn package'
   }
}
