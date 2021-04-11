# Description
Continuous integration and either continuous delivery or continuous deployment steps using Fastlane and Bitrise in iOS projects 🚀 🤝.
## Installation
- First of all you need to manage ruby version across development team and different machines using [rbenv](https://github.com/rbenv/rbenv).
    - Any mac machine  has a Local ruby version use `ruby -v` to detect it, We will work with services that interact with ruby version,
       So we need to install another ruby version in project directory it will be use across development team.
    - Install rbenv using `Homebrew` and go to project directory and install another ruby version using `rbenv local 2.6.5` for example 
- Make sure of gems (Cocoapods, Fastlane) version across development team using [bundler](https://bundler.io/).
    - Bundler is an exit from dependency hell, and ensures that the gems you need are present in development, staging, and production. 
    - Go to project directory and instal it using `gem install bundler 2.1.4` for example.
    - Use `bundler init` for determin gems version in our projects.
        - It will generate <strong> Gemfile </strong>.
            - ```ruby
                # frozen_string_literal: true

                source "https://rubygems.org"

                git_source(:github) {|repo_name| "https://github.com/#{repo_name}" }

                 gem "cocoapods", "1.10.0"
                 gem "fastlane", "2.171.0"

                plugins_path = File.join(File.dirname(__FILE__), 'fastlane', 'Pluginfile')
                eval_gemfile(plugins_path) if File.exist?(plugins_path)
                ```
    - Use `bundle install` for install specific version of our gems.
        - Any one clone our project and run `bundle install` that will install specific version of our gems.
    - Every time you will use gem use `bundle exec` before gem command.
        - for example in Cocoapods gem instead of using `pod init` we will use `bundle exec pod init`.
## Setup Project Environment 
- Setup project environments, schemes and schemes configurations (Production - Development - Testing).
    - Production environment usually use for distribute app for '`pp-store`.
    - Testing environment use for distribute app for tester using `Adhoc`.
    - Development environment use for development our features.
    - Every environment has two schemes configurations (debug - release).
        - debuge need a development provisioning profile and release need either Adhoc or appstore provisioning profile.
## Setup Fastlane
- Setup [Fastlane](http://fastlane.tools/)tool and this is the middle man 🕵 between our project and any continuous integration cloud server.
    - Add fastlane gem with specific version to gemfile and use `bundle install`.
    - Use `bundle exec fastlane init` to generate <strong>Fastfile and Appfile.</strong>. 
        - Appfile: Contains global configurations of our project for example (bundle identifier, team id and apple id).
            - ```ruby
                app_identifier("cyour app identifier") 
                 # Your Apple email address
                apple_id(" your apple id")
                team_name "team name "
                team_id "team id"
                ```
        - fastfile: Contains our lanes to automate specific develoment tasks.
 - Setup [Match Action](http://docs.fastlane.tools/actions/match/#match).
    - Share one code signing identity across your development team to simplify your codesigning setup and prevent code signing issues.
    -  Use `bundle exec fastlane match init` to generate  match file is a configuration file to match action.
       - Add <Strong> git_url,storage_mode and (true) </Strong>
        -  git_url: git private repository url that contains our certificates and provisioning profiles
        -  storage_mode: `git` or `bitbucket` for example
            - ``` ruby 
                git_url("your private repo url")

                storage_mode("git")
                ```
   - Preferred To run `match nuke `command to revoke your certificates and provisioning profiles pefore manage app signing.
    - There is two ways to setup match action <strong> Manually or Automatically</strong>. 
        - ## Match Manually Setup
            - Add app identifer and generate certificates and profiles for development, adhoc and app-store (for each scheme) in apple portal.
            - Open Xcode and make manual select for provisioning profiles and set it manually by generated ones.
            - Add and set to true readonly property in `match file` to tell fastlane you will generate certificates and provisioning profiles manually.
               - ``` ruby
                  git_url("your private repo url")
                     
                   storage_mode("git")
                     
                   readonly(true)
                   ```
         
            - Match needd cer file and  private key(.p12 file) to install certificates in aother machines.
            - Export .p12 file from keychain.
            - When you export p12 file don’t insert any password.
            - Rename cer file and p12 file with certificate id.
            - Rename Provisioning profile with  type_bundleId.mobileprovision for example .
                - AdHoc_com.your app bundle id.mobileprovision
            - Encripte certificates using ssh -> (openssl enc -aes-256-cbc -k) 
                - openssl enc  -aes-256-cbc -k "your password" -in "certificateId.cer" -out "certificateId.new.cer" -a -e -salt 
            - Encripte Provisioning profile using ssh -> (openssl enc -aes-256-cbc -k).
            - Clone certificate repo and add group structure.
            - Add folder with name 'certs' and another folder with name 'profiles' and add file match_version.txt.
                - match_version : Contain match version for example 2.171.0
          
        <p align="center">
          <img src="Screenshots/folder-structure" width="350" >
        </p>
       - ## Match Automatically Setup
            - Simple as that 😄
            - ``` ruby
                lane :beta do
                # generate certificate and Provisioning profile for appstore
                match(type: "appstore",
                app_identifier: "tools.fastlane.app")
                
                # generate certificate and Provisioning profile for development
                match(type: "development",
                app_identifier: "tools.fastlane.app")

                # generate certificate and Provisioning profile for adhoc
                match(type: "adhoc",
                      app_identifier: "tools.fastlane.app")

                 end
                 ```
 
 ## Fastlane Actions Example
 

