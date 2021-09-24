#Groups and Permissions
##Permissions
Permissions allow for the management of read/write access to various features of webapp.io. Permissions can be managed for the members of your organization through the members section of organization settings.

Permissions are a hierarchy of keywords separated by periods(`.`) using star(`*`) as a wildcard.

####Examples:

`settings.billing.tier` equates to the organziation billing tier setting permission.

`settings.*` equates to all organziation settings permissions.

`*` equates to all permissions.

###Dashboard
Access to view the organization dashboard (recent commits, custom domains, new installations, organization settings) is managed thorugh the `dashboard` keyword.

###Install
Access to install webapp.io on new repositories is managed through the `install` keyword.

###Repository
Access to perform actions on repositories is managed the the `repo` keyword.

Access to perform actions on a specific repository is managed through the `repo.<reposirory name>` keyword. Repository names are lower cased and have both spaces and periods replaced with dashes(`-`). For example, a repository `helloWorld/my first.program` is normalized to `helloWorld/my-first-program`
####Staging Terminal
Access to the debugging terminal of a staging server is managed through the `repo.<reposirory name>.stagingterm` keyword.

####Repository Controls
Repository controls are managed through the `controls` keyword with the following permissions:
- `repo.<reposirory name>.controls.retry` the ability to retry a run
- `repo.<reposirory name>.controls.cancel` the ability to cancel a run
- `repo.<reposirory name>.button` the ability to click buttons defined in Layerfiles

###Secrets
Access to read and write secrets is managed though the `secrets` keyword.

###Settings
Access to read and write organization settings are managed through the `settings` keyword.

####Membership
Organization membership management has the following permissions:
- `setttings.members.roles` alter member roles
- `setttings.members.users` alter organization's users
- `setttings.members.users.invite` invite users to organization

####Billing
Organization billing settings have the following permissions:
- `settings.billing.tier` manage organization billing tier
- `settings.billing.payment` manage organization payment method

##Groups

An organization has two role groups by default, owners and members. Owners have read/write access to all parts of the organization, members have a subset. Members start with the default permissions of:
- `dashboard`
- `repo.*.controls.cancel`
- `repo.*.controls.retry`
- `repo.*.controls.stagingterm`

Permissions can be granted to role groups in the members section of organization settings.

To add additional role groups contact support at support@webapp.io.