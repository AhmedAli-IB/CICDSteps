# CICDSteps
CICD steps using Fastlane and Bitrise  🚀 🤝

# Steps
1- Create your project repository 
2- Create your project certificates repository 
3- Add gitignore  file for your project repo
4- Make sure of ruby version in your project (Manage ruby version across development team)
    Any mac has a Local ruby version of our machine -> (ruby -v), we will work with services that interact with ruby version,
    So we need to insurance on  reuby version across development team.
    So we will use a tool called rbenv that manage ruby version across different machines.
    We will install rbenv using Homebrew and use in project directory -> (rbenv local 2.6.5 for example)
5- make sure of gems version across development team using bundler
    - Go to project directory and instal it (gem install bundler 2.1.4)
    - Use -> (bundler init) to use for determin gems version in our projects
    - Use -> (bundle install) any one clone our project and run this command that will install specific version of our gems
    - every time you will use gem use -> (bundle exec pod init) 
6- Setup project environments, schemes and schemes configurations (Production - Development - Testing)
7- fastlane automate develoment tasks (Archive - Screenshots - Code signing) this is the middle man 🕵
8- Add fastlane gem to gemfile and use -> (bundle install)
9- (bundle exec fastlane init) to generate  fastfile and appfile
   -Appfile : - contains global configurations of our project ex(bundle identifier, team id and apple id)
10 - Using match action manually 
  -> Add app identifer and generate certificates and profiles for development and adhoc  (for each scheme)
  -> Make manual selct for provisioning profiles in xcode and select your generated ones
  -> (bundle exec fastlane match init)
  match file is a configuration file to match action
  
11- Setup match folders and certificates manually 
    -> match needd cer file and  private key(.p12 file) to install certificates in aother machines
    -> when you export p12 file don’t insert any password
    -> Rename cer file and p12 file with certificate id
    -> Encripte certificates using ssh -> (openssl enc -aes-256-cbc -k)
    openssl enc  -aes-256-cbc -k "MoviePass" -in "U79NXF88P2.cer" -out "U79NXF88P2.new.cer" -a -e -salt 
12- clone certificate repo and add group structure 
(add folder with name 'certs' and 'profiles' and add file match_version.txt)
13 - Rename provisioning profile to type_bundleId.mobileprovision
  -> Take screen for folder structure 
14  - install dependencies and sign aoolication with certificates and profiles and finally archive application   
       -> To archive app we will use gem action -> (bundle exec fastlane init)
       -> wirte your scheme in gemfile and clean equal true
       -> when using gem you need to create two gem one for adhoc and another for app store
       -> note: make sure of seledted provisiong profile in xcode realse scetion befor calling lane
 15 - using poilt action to abload build to testflight 
         ->(Fastlane pilot builds) to show builds already applauded 
  
    
 
