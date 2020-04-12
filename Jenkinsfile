node {
   stage('SCM Checkout'){
     git 'https://github.com/prem-repo/hello-world'
   }
   stage('Compile-Package'){
      def mvnHome = tool name: 'mvn', type: 'maven'
      sh "${mvnHome}/bin/mvn package"
   }
}
