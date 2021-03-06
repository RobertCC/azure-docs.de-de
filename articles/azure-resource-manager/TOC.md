# Übersicht
## [Was ist Resource Manager?](resource-group-overview.md)
## [Ressourcenanbieter und -typen](resource-manager-supported-services.md)
## [Resource Manager-Bereitstellung und klassische Bereitstellung](resource-manager-deployment-model.md)
## [Abonnementgovernance](resource-manager-subscription-governance.md)
## [Verwaltete Anwendungen](managed-application-overview.md)

# Erste Schritte
## [Exportieren der Vorlage](resource-manager-export-template.md)
## [Erstellen und Bereitstellen der Vorlage](resource-manager-create-first-template.md)
## [Visual Studio mit Resource Manager](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)

# Beispiele
## [Codebeispiele](https://azure.microsoft.com/en-us/resources/samples/?service=azure-resource-manager)
## PowerShell
### [Bereitstellen der Vorlage](resource-manager-samples-powershell-deploy.md)

## Azure-Befehlszeilenschnittstelle
### [Bereitstellen der Vorlage](resource-manager-samples-cli-deploy.md)

# Anleitung
## Erstellen von Vorlagen
### [Bewährte Methoden für Vorlagen](resource-manager-template-best-practices.md)
### [Vorlagenabschnitte](resource-group-authoring-templates.md)
### [Verknüpfen mit anderen Vorlagen](resource-group-linked-templates.md)
### [Definieren der Abhängigkeit zwischen Ressourcen](resource-group-define-dependencies.md)
### [Erstellen mehrerer Instanzen](resource-group-create-multiple.md)
### [Standort festlegen](resource-manager-template-location.md)
### [Tags zuweisen](resource-manager-template-tags.md)
### [Festlegen von Name und Typ der untergeordneten Ressource](resource-manager-template-child-resource.md)
### [Aktualisieren von Ressourcen](resource-manager-update.md)
### [Verwenden von Objekten für Parameter](resource-manager-objects-as-parameters.md)
### [Freigeben des Status für verknüpfte Vorlagen](best-practices-resource-manager-state.md)
### [Muster für das Entwerfen von Vorlagen](best-practices-resource-manager-design-templates.md)

## Bereitstellen
### PowerShell
#### [Bereitstellen der Vorlage](resource-group-template-deploy.md)
#### [Bereitstellen einer privaten Vorlage mit SAS-Token](resource-manager-powershell-sas-token.md)
#### [Exportieren der Vorlage und erneutes Bereitstellen](resource-manager-export-template-powershell.md)
### Azure-Befehlszeilenschnittstelle
#### [Bereitstellen der Vorlage](resource-group-template-deploy-cli.md)
#### [Bereitstellen einer privaten Vorlage mit SAS-Token](resource-manager-cli-sas-token.md)
#### [Exportieren der Vorlage und erneutes Bereitstellen](resource-manager-export-template-cli.md)
### [Portal](resource-group-template-deploy-portal.md)
### [REST-API](resource-group-template-deploy-rest.md)
### [Ressourcengruppenübergreifende Bereitstellung](resource-manager-cross-resource-group-deployment.md)
### [Continuous Integration in Visual Studio Team Services](../vs-azure-tools-resource-groups-ci-in-vsts.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
### [Übergeben sicherer Werte während der Bereitstellung](resource-manager-keyvault-parameter.md)

## Verwalten
### [PowerShell](powershell-azure-resource-manager.md)
### [Azure-Befehlszeilenschnittstelle](xplat-cli-azure-resource-manager.md)
### [Portal](resource-group-portal.md)
### [REST-API](resource-manager-rest-api.md)
### [Verwenden von Tags zum Organisieren von Ressourcen](resource-group-using-tags.md)
### [Verschieben von Ressourcen in neue Gruppen oder Abonnements](resource-group-move-resources.md)
### [Governancebeispiele](resource-manager-subscription-examples.md)

## Steuern des Zugriffs
### Erstellen eines Dienstprinzipals
#### [PowerShell](resource-group-authenticate-service-principal.md)
#### [Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
#### [Azure CLI 1.0](resource-group-authenticate-service-principal-cli.md)
#### [Portal](resource-group-create-service-principal-portal.md)
### [Authentifizierungs-API für den Zugriff auf Abonnements](resource-manager-api-authentication.md)
### [Sperren von Ressourcen](resource-group-lock-resources.md)

## Festlegen von Ressourcenrichtlinien
### [Was sind Ressourcenrichtlinien?](resource-manager-policy.md)
### [Zuweisen von Richtlinien über das Portal](resource-manager-policy-portal.md)
### [Zuweisen von Richtlinien über Skripts](resource-manager-policy-create-assign.md)
### Beispiele
#### [Tags](resource-manager-policy-tags.md)
#### [Benennungskonventionen](resource-manager-policy-naming-convention.md)
#### [Netzwerk](resource-manager-policy-network.md)
#### [Speicher](resource-manager-policy-storage.md)
#### [Linux-VM](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
#### [Windows-VM](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)

## Verwenden verwalteter Anwendungen
### [Veröffentlichen der Dienstkataloganwendung](managed-application-publishing.md)
### [Nutzen der Dienstkataloganwendung](managed-application-consumption.md)
### [Veröffentlichen der Marketplace-Anwendung](managed-application-author-marketplace.md)
### [Nutzen der Marketplace-Anwendung](managed-application-consume-marketplace.md)
### [Erstellen von Benutzeroberflächen-Definitionen](managed-application-createuidefinition-overview.md)

## Audit
### [Anzeigen von Aktivitätsprotokollen](resource-group-audit.md)
### [Bereitstellungsvorgänge anzeigen](resource-manager-deployment-operations.md)

## Problembehandlung
### [Häufige Bereitstellungsfehler](resource-manager-common-deployment-errors.md)
### [Grundlegendes zu Bereitstellungsfehlern](resource-manager-troubleshoot-tips.md)
### [RequestDisallowedByPolicy-Fehler](resource-manager-policy-requestdisallowedbypolicy-error.md)
### Bereitstellungsfehler bei virtuellen Computern
#### [Linux](../virtual-machines/linux/troubleshoot-deploy-vm.md)
#### [Windows](../virtual-machines/windows/troubleshoot-deploy-vm.md)

# Referenz
## [Vorlagenformat](/azure/templates/)
## [Vorlagenfunktionen](resource-group-template-functions.md)
### [Array- und Objektfunktionen](resource-group-template-functions-array.md)
### [Vergleichsfunktionen](resource-group-template-functions-comparison.md)
### [Bereitstellen von Funktionen](resource-group-template-functions-deployment.md)
### [Logische Funktionen](resource-group-template-functions-logical.md)
### [Numerische Funktionen](resource-group-template-functions-numeric.md)
### [Ressourcenfunktionen](resource-group-template-functions-resource.md)
### [Zeichenfolgenfunktionen](resource-group-template-functions-string.md)
## [Funktionen von Benutzeroberflächen-Definitionen](managed-application-createuidefinition-functions.md)
## [Elemente für Benutzeroberflächendefinitionen](managed-application-createuidefinition-elements.md)
### [Microsoft.Common.DropDown](managed-application-microsoft-common-dropdown.md)
### [Microsoft.Common.FileUpload](managed-application-microsoft-common-fileupload.md)
### [Microsoft.Common.OptionsGroup](managed-application-microsoft-common-optionsgroup.md)
### [Microsoft.Common.PasswordBox](managed-application-microsoft-common-passwordbox.md)
### [Microsoft.Common.Section](managed-application-microsoft-common-section.md)
### [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md)
### [Microsoft.Compute.CredentialsCombo](managed-application-microsoft-compute-credentialscombo.md)
### [Microsoft.Compute.SizeSelector](managed-application-microsoft-compute-sizeselector.md)
### [Microsoft.Compute.UserNameTextBox](managed-application-microsoft-compute-usernametextbox.md)
### [Microsoft.Network.PublicIpAddressCombo](managed-application-microsoft-network-publicipaddresscombo.md)
### [Microsoft.Network.VirtualNetworkCombo](managed-application-microsoft-network-virtualnetworkcombo.md)
### [Microsoft.Storage.MultiStorageAccountCombo](managed-application-microsoft-storage-multistorageaccountcombo.md)
### [Microsoft.Storage.StorageAccountSelector](managed-application-microsoft-storage-storageaccountselector.md)
## [PowerShell](/powershell/module/azurerm.resources)
## [Azure-Befehlszeilenschnittstelle](/cli/azure/resource)
## [.NET](/dotnet/api/microsoft.azure.management.resourcemanager)
## [Java](/java/api/com.microsoft.azure.management.resources)
## [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagement.html)
## [REST](/rest/api/resources/)

# Ressourcen
## [Azure-Roadmap](https://azure.microsoft.com/roadmap/?category=monitoring-management)
## [Preisrechner](https://azure.microsoft.com/pricing/calculator/)
## [Dienstupdates](https://azure.microsoft.com/updates/?product=azure-resource-manager)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-resource-manager)
## [Drosselungsanforderungen](resource-manager-request-limits.md)
## [Nachverfolgen asynchroner Vorgänge](resource-manager-async-operations.md)
## [Videos](https://azure.microsoft.com/documentation/videos/index/?services=azure-resource-manager)
