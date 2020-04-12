node {
   stage('SCM Checkout'){
     git 'https://github.com/prem-repo/hello-world'
   }
   stage('Compile-Package'){
    sh 'mvn package'
   }
}
