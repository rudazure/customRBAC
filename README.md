# customRBAC
how to create a custom role (RBAC) on Azure. That is only a way to do it.

#### log on Azure acc
    Connect-AzAccount

#### get the list of roles
    Get-AzRoleDefinition | FT Name, IsCustom

#### get details of such a role (just an example)
    Get-AzRoleDefinition "Support Request Contributor" | ConvertTo-Json

#### get the ctions of such a role
    (Get-AzRoleDefinition "Storage Contributor").Actions

Create a file with the notation below in this example I created the new line: "Microsoft.Network/dnszones/*" as notAction (avoid that usage)
Content below was copied/pasted at vscode to create a file
##### PAY ATTENTION: 
the name of the new role is passed through the line "Name", below.
Also, consider "IsCustom" as true
in the fiel "Actions" you will paste all Actions you got from the built-in or existent role
FINALLY, you will add new actions OR remove some actions by using the field "NotActions"


    {
        "Name":  "Website YourCompany",
        "Id":  "null",
        "IsCustom":  true,
        "Description":  "custom website role to avoid some support actions",
        "Actions":  [

            "Microsoft.Authorization/*/read",
            "Microsoft.Insights/alertRules/*",
            "Microsoft.Insights/components/*",
            "Microsoft.ResourceHealth/availabilityStatuses/read",
            "Microsoft.Resources/deployments/*",
            "Microsoft.Resources/subscriptions/resourceGroups/read",
            "Microsoft.Support/*",
            "Microsoft.Web/certificates/*",
            "Microsoft.Web/listSitesAssignedToHostName/read",
            "Microsoft.Web/sites/*"

                    ],
        "NotActions":  [

                        "Microsoft.Support/supportTickets/read"

                       ],
        "DataActions":  [

                        ],
        "NotDataActions":  [

                           ],
        "AssignableScopes":  [
                                 "/subscriptions/8c555d04-5295-4d84-xxxx-89380519836d"
                             ]
    }


#### create the new role based on the file created
    New-AzRoleDefinition -InputFile "C:\Customrole\supportcustom-cantread.json"
