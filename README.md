## Template Repository
  ##### There will be 3 projects in a connector repository.
  - 1 parent project and 2 subprojects.
  - one Subproject should be created for connector provider and one for providerExt.
   

  ##### Parent project
  
  - .github folder contains workflows. These workflows should be configured to run on pull request to master. All yml files under this folder will be executed based on condition. 
  <br><br>start should contain below lines.
           
           
            on:
              pull_request:
                branches: [ master ]     
   <br>yml file should have below commands at the end to publish the project to Github build snapshot.
   
            - name: Build with Gradle
                  env:
                    GH_SECRET_TOKEN: ${{ secrets.GH_SECRET_TOKEN }}
                    GH_RUN_NUMBER: ${{ github.run_number }}
                    
            
                  run: |
                    gradle :WmTemplateProviderExt:assembleArtifact
                    gradle :WmTemplateProvider:assembleArtifact
                    gradle publishGprPublicationToSNAPSHOTRepository
   <br>
   
   - For any connector related documentation use "documentation" folder in parent project.
   - Test assets should be stored inside "test" folder.
   - gradle.properties should be used for all properties related to build.
   <br><br>
     
   Remember to update below properties after cloning this Template package. 
    
            repo.name=connector-template
            build.version.major=10
            build.version.minor=5
            build.version.micro=0
            provider.package.name=WmTemplateProvider
            providerext.package.name=WmTemplateProviderExt
            bas.provider.name=bas_providername
            bas.provider.build.name=buildname
            bas.providerext.build.name=buildname
    
            
   - use ".gitignore" to ignore any files/folder you may not want to commit.
   <br><nbsp> example- build, .gradle, secrets.properties, .project, .idea
   
   - secrets.properties should be used to store secret tokens. SO PLEASE REMEMBER TO ADD secrets.properties TO .gitignore
   - settings.gradle contains root project and subproject names.
  
  <br><br>
  #### Sub Projects
  
  - Parent project contains two subprojects under the folder 'packages'.  
  - Both child projects contain build.gradle and settings.gradle.
  - Developer should update these build.gradle files to add dependency required for the connector.
  - Be cautious while adding a dependency. Please do not add redundant dependency to the project.
  
  ### Steps to be followed
  
  1. Use this template repository to create repo for your connector.
      Name should be "connector-<your connector name in all lower case seperated by"-">"
  1. Create develop and feature(it should be id of itrac you are working on) branch from master.
  1. Clone feature branch in your local.
  1. Steps to untrack secret.properties file. If this file is not present on your local, you can skip this step.
    These steps are required as we don't want to expose personal secret tokens on GitHub repo. 
      1. Now we need to remove secrets.properties file from GitHub. To do this take a backup of this file and delete from the project.
      1. Commit and push your changes to feature.
      1. Add secrets.properties back to the project. Make sure secrets.properties is added to .gitignore. Add your secret token to this file. This will allow you to build packages locally. 
  1. Now open gradle.properties and update below properties accordingly.<br>
    
              repo.name=connector-template
              build.version.major=10
              build.version.minor=5
              build.version.micro=0
              provider.package.name=WmTemplateProvider
              providerext.package.name=WmTemplateProviderExt
              bas.provider.name=bas_providername
              bas.provider.build.name=buildname
              bas.providerext.build.name=buildname
              
  1. Run "generateArtifacts.bat" script. This script will generate the basic folder structure and update settings <br>
            
            generateArtifacts.bat

  1. Add contents of your provider and ext package to these packages accordingly. <br> To get clean package from SVN run below command. Replace svn url accordingly.
                
                svn export http://svndae.eur.ad.sag:1818/svn/sag/cloudstreams/providers/adobe/trunk
                
  1. Please do not delete build.gradle and settings.gradle in any of the folders.
  1. build.gradle in subProject should be updated if needed to add any dependency.
  1. Once changes are merged to master GitHub Actions workflow will be invoked and packages will be published to "System-Build-Snapshot" https://github.com/orgs/webMethods-Connectors/packages?repo_name=System-Build-Snapshot.

  1. Always use feature brach for development. Merge your changes from feature to develop branch. After testing raise pull request to master. A reviewer needs to review and merge the changes.
 1. Steps to build locally:
 
       1. Run below commands to generate package zip locally.<br>
        
                  gradle :WmTemplateProviderExt:assembleArtifact
                  gradle :WmTemplateProvider:assembleArtifact          
       1. You will find zip in "build/tmpzip" folder.
       
### Common issues

   1. Long path issue: If file path inside git repository is too long you may get error "Filename too long"
   <br> Resolution: navigate to your repository and open .git/config file and add "longpaths = true" in core section.
                
                [core]
                    longpaths = true    
   1. While cloning if you get error "fatal: not a git repository".
    <br>Resolution
        1. Make sure link to repository is copied properly.
        1. On github make sure you have proper access.
        1. If none of above resolutions work, try reconfiguring user in your local git. To do this run below command.
                        
                        git credential-manager delete https://github.com 
            Then try to clone your repository once again and provide proper credential when prompted.   
 
                
                
       
                