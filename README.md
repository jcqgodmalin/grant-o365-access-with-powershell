# Grant your app O365 access with powershell

Grant the JICAuthenticator app full access to the O365 mailbox steps:

1.	Open PowerShell as administrator.
2.	Run the command `Import-Module AzureADPreview`

  	2.1.	If you encounter an error like this, this means that the module AzureAD is not yet installed. Refer to the Module Installation guide below.
  
		`import-module : The specified module 'AzureADPreview' was not loaded because no valid module file was found in any module directory.`

    After the installation, run the import command (`Import-Module AzureADPreview`) again.

  	2.2.	If you encounter the same error above even after installation, just import `AzureAD` instead of `AzureADPreview` (Ex: `Import-Module AzureAD`)

3.	Run the command `connect-azuread`

  	You will be prompt with a login box. Sign in to account that has Global Administrator role in the tenant you want to connect.
 
4.	Run the command `$myapp = Get-AzureADServicePrincipal -SearchString <app-name>`

5.	Run the command `$cred = Get-credential`

  	You will be prompt with a login box again. Sign in to the same account you used in Step 3.
 
6.	Run the command `$session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://ps.outlook.com/powershell/?SerializationLevel=Full -Credential $cred -Authentication Basic -AllowRedirection`

7.	Run the command `Import -PSSession $session -Prefix o365`

8.	Run the command `New-o365serviceprincipal -AppId $myapp.AppId -ServiceId $myapp.ObjectId -DisplayName “Service Principal for <app-name>”`

9.	Run the command `Add-o365MailboxPermission -Identity <mailbox email> -User $myapp.ObjectId -AccessRights Full`


# Module Installation Steps:

1	Open PowerShell as administrator.

2	Enter the command `Install-module -Name ExchangeOnlineManagement -allowprerelease`

  	2.1.	In case you encounter an error here like this, just remove the `-allowprerelease` parameter

    		`Install-Module : A parameter cannot be found that matches parameter name 'allowprerelease'.`

  	2.2.	Enter `Y` to install the NuGet Provider as it is required to continue the module installation.

  	2.3.	Enter `A` to select the Yes to all option when prompted that you’re installing modules from untrusted repository.

3	Enter the command `Install-module -Name AzureAD`

  	3.1.	Enter `A` to select the Yes to all option when prompted that you’re installing modules from untrusted repository.
