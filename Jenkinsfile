node {
  stage('init') {
    checkout scm
  }
  
  stage('deploy') {
    def gitUser="glhcdeployUser"
    def gitPassword="deployPass"
    
    // login Azure
    withCredentials([azureServicePrincipal('azure_service_principal')]) {
      sh '''
        az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
        az account set -s $AZURE_SUBSCRIPTION_ID
      '''
    }
    
    // deploy/update app
    sh "az webapp deployment user set --user-name $gitUser --password $gitPassword"
    sh "git remote add azure https://$gitUser:$gitPassword@gldotnetsqlapp.scm.azurewebsites.net/gldotnetsqlapp.git"
    sh '''
      git add .
      git commit -m "added done field"
      git push azure master
    '''
    
    // logout Azure
    sh 'az logout'
  }
}
