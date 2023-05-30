## Azure Policy through powershell
```powershell
#get resource group
$rg = get-azresourcegroup -name "rg-d11n-policy"
$definition = Get-AzPolicyDefinition -Id "/providers/Microsoft.Authorization/policyDefinitions/4f9dc7db-30c1-420c-b61a-e1d640128d26"
new-azPolicyAssignment - Name "new tag" -policyDefinition $definition -Scop $rg.ResourceID
```
