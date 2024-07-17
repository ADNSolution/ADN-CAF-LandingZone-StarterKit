# ADN CAF-LandingZone-StarterKit

## Wofür brauche ich das Landing Zone StarterKit

Es gibt viele Abonnements in Azure, bei denen eine grundlegende Governance fehlt. Die Absicht des Repos ist es, einige grundlegende Governance-Einstellungen für ein Abonnement bereitzustellen. Dies ist kein Ersatz für eine gut gestaltete Azure Landing Zone. Wenn Sie die Reise zu einer Azure Landing Zone beginnen möchten, lesen Sie bitte diesen [Artikel über das Cloud Adoption Framework(CAF)](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/).

Wenn Sie nicht mit der auf dem CAF basierenden Lannding-Zone beginnen, wird Ihnen dieses Repository dabei helfen, einige Grundlagen zu schaffen, um ein Minimum an Governance einzurichten. 

### Cloud governance Disziplinen 

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

##### Einrichten der Budget-Warnung

Dieses Repository bietet ein Modul zur Bereitstellung der Budget-Warnung. Die Parameter für diesen Alarm werden alle in der Datei azskmain.parameters.json im Stammverzeichnis des Repositorys festgelegt. Die folgenden Parameter müssen gesetzt werden:


|Parameter|Beschreibung|Standardwert|
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

##### Identitäts- und Zugangsmanagement
|Policy|Descitpion|Referenz|
|---|---|---|
|Um sicherzustellen, dass die zuständigen Personen in Ihrem Unternehmen benachrichtigt werden, wenn eine potenzielle Sicherheitsverletzung in einem Ihrer Abonnements auftritt, legen Sie einen Sicherheitskontakt fest, der E-Mail-Benachrichtigungen vom Sicherheitscenter erhält.|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_Security_contact_email.json)|
|Für Ihr Abonnement sollten maximal 3 Eigentümer bestimmt werden. Es wird empfohlen, bis zu 3 Eigentümer für Ihr Abonnement zu bestimmen, um das Potenzial für einen Verstoß durch einen gefährdeten Eigentümer zu verringern.|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_DesignateLessThanXOwners_Audit.json)|

##### Netzwerk
|Policy|Descitpion|Referenz|
|---|---|---|
|Alle Netzwerk-Ports sollten auf Netzwerksicherheitsgruppen beschränkt werden, die mit Ihren VMs verknüpft sind|Alle Netzwerk-Ports sollten auf Netzwerksicherheitsgruppen beschränkt werden, die mit Ihrer virtuellen Maschine verknüpft sind|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_UnprotectedEndpoints_Audit.json)|
|nternetfacing VMs sollten mit NSGs geschützt werden'|Schützen Sie Ihre virtuellen Maschinen vor potenziellen Bedrohungen, indem Sie den Zugriff auf sie mit Netzwerksicherheitsgruppen (NSGs) beschränken. Erfahren Sie mehr über die Kontrolle des Datenverkehrs mit NSGs unter [https://aka.ms/nsg-doc](https://aka.ms/nsg-doc)|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_NetworkSecurityGroupsOnInternetFacingVirtualMachines_Audit.json)|
|Subnetze sollten mit einer Netzwerksicherheitsgruppe verbunden werden. Schützen Sie Ihr Subnetz vor potenziellen Bedrohungen, indem Sie den Zugriff darauf mit einer Netzwerksicherheitsgruppe (NSG) einschränken. NSGs enthalten eine Liste von ACL-Regeln (Access Control List), die den Netzwerkverkehr zu Ihrem Subnetz erlauben oder verweigern.|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_NetworkSecurityGroupsOnSubnets_Audit.json)|
|Network Watcher sollte aktiviert sein. Network Watcher ist ein regionaler Dienst, der es Ihnen ermöglicht, Bedingungen auf der Ebene eines Netzwerkszenarios in, zu und von Azure zu überwachen und zu diagnostizieren. Die Überwachung auf Szenarioebene ermöglicht Ihnen die Diagnose von Problemen auf einer End-to-End-Ansicht auf Netzwerkebene. In jeder Region, in der ein virtuelles Netzwerk vorhanden ist, muss eine Netzwerküberwachungsressourcengruppe erstellt werden. Wenn in einer bestimmten Region keine Netzwerküberwachungs-Ressourcengruppe verfügbar ist, wird eine Warnung aktiviert.|[Defintion](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/NetworkWatcher_Enabled_Audit.json)|

##### Sicherheit
|Policy|Descitpion|Referenz|
|---|---|---|
|System-Updates sollten auf Ihren Rechnern installiert werden. Fehlende Sicherheitssystem-Updates auf Ihren Servern werden vom Azure Security Center als Empfehlungen überwacht.|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_MissingSystemUpdates_Audit.json)|

Alle Richtlinien werden in einer Initiative gesammelt und diese wird dem Abonnement im Rahmen der Bereitstellung zugewiesen. Dies bietet die Möglichkeit, auf einen Blick zu überprüfen, wie gut die Azure-Ressourcen mit diesen Richtlinien übereinstimmen.

![Ansicht zur Einhaltung von Azure-Richtlinien](/media/AuditReport.png)

## Wie wird das Paket ausgerollt?

Um dieses Starter Kit für ein neues oder bereits bestehendes Abonnement einzusetzen, führen Sie einfach die folgenden Schritte aus:

1. Sicherstellen, dass Sie bei Azure angemeldet sind

    ```azurecli
    az login
    ```

2. Auswählen der richtigen Subscrition

    ```azurecli
    az account show --output table
    ```

    Prüfen Sie in der Ausgabe, welches Abonnement verwendet werden soll, und führen Sie den folgenden Befehl aus, um Ihre spezifische ID festzulegen

    ```azurecli
    $subscriptionID = "your subscription ID"
    ``` 

    Einrichten des Kontos für die Verwendung der Verdächtigung

    ```azurecli
    az account set --subscription $subscriptionID
    ```

3. Im nächsten Schritt können Sie zwischen zwei Optionen wählen:

- Erstellen Sie eine neue Bereitstellung auf der Abonnementebene mit der klassischen **.json-Parameterdatei**, um das Starter Kit bereitzustellen

    ```azurecli
    $location = "your preferred location"
    
    az deployment sub create --location $location --template-file "azskmain.bicep" --parameters "azskmain.parameters.json" --confirm-with-what-if
    
    ```

- Erstellen Sie eine neue Bereitstellung auf der Abonnementebene mit der neuen **.bicepparam-Parameterdatei**, um das Starterkit bereitzustellen

    Zu diesem Zweck müssen Sie eine neue Bicep-Datei mit dem Namen azskmain.bicepparam erstellen, um die Parameter für die Bereitstellung zu definieren. Um diese Art von Dateien zu verwenden, müssen Sie die folgenden Versionen auf Ihrem System haben:

  - Azure CLI 2.48.1 oder später (check with az --version)
  - Bicep version 0.16.2 oder später (chekc with az bicep --version)
  - und Sie müssen Ihre bicepconfig.json konfigurieren (siehe Repo für ein Beispiel)

    ```azurecli
    $location = "your preferred location"
    
    az deployment sub create --location $location --template-file "azskmain.bicep" --parameters "azskmain.bicepparam" --confirm-with-what-if

    ```
  
## Beitragender

Dieses Projekt begrüßt Beiträge und Vorschläge.  Die meisten Beiträge erfordern die Zustimmung zu einer
Contributor License Agreement (CLA) zustimmen, in dem Sie erklären, dass Sie das Recht haben, uns die Rechte zur Nutzung Ihres Beitrags einzuräumen, und dies auch tatsächlich tun.
die Rechte zur Nutzung Ihres Beitrags einräumen. Einzelheiten finden Sie unter <https://cla.microsoft.com.>.

Wenn Sie einen Pull Request einreichen, wird ein CLA-Bot automatisch feststellen, ob Sie eine CLA vorlegen müssen
eine CLA angeben müssen und schmückt den PR entsprechend aus (z.B. Label, Kommentar). Befolgen Sie einfach die Anweisungen
des Bots. Sie müssen dies nur einmal für alle Repos tun, die unser CLA verwenden.

Dieses Projekt hat den [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) angenommen.
Weitere Informationen finden Sie in den [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) oder
kontaktieren Sie [opencode@microsoft.com](mailto:opencode@microsoft.com) mit zusätzlichen Fragen oder Kommentaren.
