# scepter-vsts-build-release-definitions

[![scepter-logo](http://res.cloudinary.com/source-4-society/image/upload/v1519221119/scepter_hzpcqt.png)](https://github.com/source4societyorg/SCEPTER-core)

This repository contains useful build definitions that can be imported into [Visual Studio Team Services][https://visualstudio.com/vso] build and release for your [SCEPTER](https://github.com/source4societyorg/SCEPTER-Core] project.

# ui-s3-deployment

This build definition will automatically run the unit tests associated with your web user interface and proceed to upload them to the S3 bucket in your definition. For this script to work, you will need to setup the branch in the core repository that will automatically trigger the build, as well as the file path to the user interface that you wish to publish (so that when a change is made to your ui application in the core, it will automatically publish).

Furthermore you will need to configure the specific environment targets and bucket destination on the individual tasks within the definition.

## setup

Requires the [Yarn task by Geek Learning](https://marketplace.visualstudio.com/items?itemName=geeklearningio.gl-vsts-tasks-yarn)

Set the following variables up in the variables tab:

    scepter.branch        # the branch you would like to target for automatic build triggers, ex. integration
    scepter.environment   # the specific configuration environment to trigger, ex. development
    scepter.pat           # your VSTS base64 encoded PAT token. [See this article for help](https://blog.devmatter.com/personal-access-tokens-and-vsts-apis/)
    scepter.repositoryurl # The URL of the repository you wish to send the authentication header to, ex. scepter.visualstudio.com
    scepter.uifolder      # the folder name of the user interface you wish to target in your projects ui folder, ex. webui
    
  