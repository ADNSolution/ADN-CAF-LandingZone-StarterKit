# ADN CAF-LandingZone-StarterKit

## Wofür brauche ich das Landing Zone StarterKit

Es gibt viele Abonnements in Azure, bei denen eine grundlegende Governance fehlt. Die Absicht des Repos ist es, einige grundlegende Governance-Einstellungen für ein Abonnement bereitzustellen. Dies ist kein Ersatz für eine gut gestaltete Azure Landing Zone. Wenn Sie die Reise zu einer Azure Landing Zone beginnen möchten, lesen Sie bitte diesen [Artikel über das Cloud Adoption Framework(CAF)](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/).

Wenn Sie nicht mit der auf dem CAF basierenden Lannding-Zone beginnen, wird Ihnen dieses Repository dabei helfen, einige Grundlagen zu schaffen, um ein Minimum an Governance einzurichten. 

### Cloud governance disciplins

Fünf Disziplinen der Cloud-Governance: 

Diese Disziplinen unterstützen die Unternehmenspolitik. Jede Disziplin schützt das Unternehmen vor möglichen Fallstricken:

- Disziplin Kostenmanagement
- Sicherheitsgrundlagen-Disziplin
- Disziplin Ressourcenkonsistenz
- Identitäts-Grundlagendisziplin
- Disziplin Bereitstellungsbeschleunigung

Dieses Starterkit hilft bei der Einrichtung der Grundlagen für Kostenmanagement, Sicherheit und Identität.

#### Kostenmanagement

Die Mindestmaßnahme, die im Rahmen eines Abonnements in Azure zu ergreifen ist, besteht darin, dass in Azure Cost Management zwei grundlegende Warnungen eingerichtet werden sollten. Einer dient der Erkennung eines festgelegten Budgets und der andere der Erkennung von Anomalien im Verbrauch.

##### Einrichten der Haushaltswarnung

Dieses Repository bietet ein Modul zur Bereitstellung der Haushaltswarnung. Die Parameter für diesen Alarm werden alle in der Datei azskmain.parameters.json im Stammverzeichnis des Repositorys festgelegt. Die folgenden Parameter müssen gesetzt werden:


|Parameter|Descitpion|Default Value|
|---|---|---|
|azskBudgetname|Der benutzerfreundliche Name der Budget-Warnung|'Azure StarterKit Budget'|
|azskBudgetAmount|Der Budgetwert, gegen den getestet werden soll. Sie legen den Geldbetrag fest, auf den der Alarm reagieren soll. Wenn Sie den Wert auf 150 setzen, wird der Alarm ausgelöst, sobald die Ausgaben den prozentualen Anteil des primären und sekundären Schwellenwerts erreichen.
|azskEmailsForAlert|E-Mail-Adresse, an die der alert gesendet werden soll|no default|
|azskBudgetStartDate|Das Startdatum für die Prüfung der aktuellen Kosten anhand der Budgetgrenze|'2022-12-01'|
|azskBudgetEndDate|Das Enddatum dieses Kosten-/Budgettests.|'2025-12-01|
|azskBudgetFirstThreshold|Wenn dieser Prozentsatz erreicht ist, wird der erste Alarm gesendet (80).
|azskBudgetSecondThreshold|der zweite prozentuale Schwellenwert für die Auslösung des Alarms|100|

##### Anomalie-Alarm

Im Azure Cost Management gibt es die Möglichkeit, einen Anomaliealarm einzurichten. Mit dieser Einstellung wird eine E-Mail verschickt, sobald Azure Cost Management eine Anomalie in der Kostenprognose feststellt. Es können mehrere Alarme im Portal eingerichtet werden. 

#### Grundlegende Sicherheit

Wir richten einige grundlegende Azure-Richtlinien ein. Dies ist die Liste der Richtlinien, die dem Abonnement zugewiesen werden. Die folgenden Tabellen zeigen die Gruppen und die zugewiesenen Richtlinien.

##### Identiy and Accessmanagement
|Policy|Descitpion|Referenz|
|---|---|---|
|Subscription should have a contact Email address for security issues|To ensure the relevant people in your organization are notified when there is a potential security breach in one of your subscriptions, set a security contact to receive email notifications from Security Center.|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_Security_contact_email.json)|
|A maximum of 3 owners should be designated for your subscription|It is recommended to designate up to 3 subscription owners in order to reduce the potential for breach by a compromised owner|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_DesignateLessThanXOwners_Audit.json)|

##### Network
|Policy|Descitpion|Referenz|
|---|---|---|
|All network ports should be restricted on network security groups associated with your VMs|All network ports should be restricted on network security groups associated to your virtual machine|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_UnprotectedEndpoints_Audit.json)|
|nternetfacing VMs should be protected with NSGs'|Protect your virtual machines from potential threats by restricting access to them with network security groups (NSG). Learn more about controlling traffic with NSGs at [https://aka.ms/nsg-doc](https://aka.ms/nsg-doc)|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_NetworkSecurityGroupsOnInternetFacingVirtualMachines_Audit.json)|
|Subnets should be associated with a Network Security Group|Protect your subnet from potential threats by restricting access to it with a Network Security Group (NSG). NSGs contain a list of Access Control List (ACL) rules that allow or deny network traffic to your subnet.|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_NetworkSecurityGroupsOnSubnets_Audit.json)|
|Network Watcher should be enabled|Network Watcher is a regional service that enables you to monitor and diagnose conditions at a network scenario level in, to, and from Azure. Scenario level monitoring enables you to diagnose problems at an end to end network level view. It is required to have a network watcher resource group to be created in every region where a virtual network is present. An alert is enabled if a network watcher resource group is not available in a particular region.|[Defintion](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/NetworkWatcher_Enabled_Audit.json)|

##### Security
|Policy|Descitpion|Referenz|
|---|---|---|
|System updates should be installed on your machines|Missing security system updates on your servers will be monitored by Azure Security Center as recommendations|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_MissingSystemUpdates_Audit.json)|

All policies will be collection into a initiative and this will be assigned to the subscription in scope of the deployment. This gives the option to check how good the Azure resources are compliant to these policies in one view.

![Azure Policy compliance view](/media/AuditReport.png)

## How to deploy

To deploy this Starter Kit to a new or already existing subscription just follow these steps:

1. Ensure to be loged into Azure

    ```azurecli
    az login
    ```

2. Select the right Subscrition

    ```azurecli
    az account show --output table
    ```

    Check the output for the subscription which should be used and run the following comamnd to set your specific ID

    ```azurecli
    $subscriptionID = "your subscription ID"
    ``` 

    Set the account to use the susbcription

    ```azurecli
    az account set --subscription $subscriptionID
    ```

3. In the next step you can choose between to option:

- Create a new deployment at Suscription level with the classic **.json parameter file** to deploy the Starter Kit

    ```azurecli
    $location = "your preferred location"
    
    az deployment sub create --location $location --template-file "azskmain.bicep" --parameters "azskmain.parameters.json" --confirm-with-what-if
    
    ```

- Create a new deployment at Suscription level with the new **.bicepparam parameter file** to deploy the Starter Kit

    To do this, you need to create a new bicep file with the name azskmain.bicepparam to define the parameters for the deployment. To use this kind of files you need to have the following versions on your system:

  - Azure CLI 2.48.1 or later (check with az --version)
  - Bicep version 0.16.2 or later (chekc with az bicep --version)
  - and you have to configure your bicepconfig.json (see repo for an example)

    ```azurecli
    $location = "your preferred location"
    
    az deployment sub create --location $location --template-file "azskmain.bicep" --parameters "azskmain.bicepparam" --confirm-with-what-if

    ```
  
## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit <https://cla.microsoft.com.>

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
