---
title: Complete mode deletion
description: Shows how resource types handle complete mode deletion in Azure Resource Manager templates.
ms.topic: conceptual
ms.date: 10/20/2021
---

# Deletion of Azure resources for complete mode deployments

This article describes how resource types handle deletion when not in a template that is deployed in complete mode.

The resource types marked with **Yes** are deleted when the type isn't in the template deployed with complete mode.

The resource types marked with **No** aren't automatically deleted when not in the template; however, they're deleted if the parent resource is deleted. For a full description of the behavior, see [Azure Resource Manager deployment modes](deployment-modes.md).

If you deploy to [more than one resource group in a template](./deploy-to-resource-group.md), resources in the resource group specified in the deployment operation are eligible to be deleted. Resources in the secondary resource groups aren't deleted.

The resources are listed by resource provider namespace. To match a resource provider namespace with its Azure service name, see [Resource providers for Azure services](../management/azure-services-resource-providers.md).

> [!NOTE]
> Always use the [what-if operation](./deploy-what-if.md) before deploying a template in complete mode. What-if shows you which resources will be created, deleted, or modified. Use what-if to avoid unintentionally deleting resources.

Jump to a resource provider namespace:
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [Microsoft.Addons](#microsoftaddons)
> - [Microsoft.ADHybridHealthService](#microsoftadhybridhealthservice)
> - [Microsoft.Advisor](#microsoftadvisor)
> - [Microsoft.AgFoodPlatform](#microsoftagfoodplatform)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.AnyBuild](#microsoftanybuild)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppAssessment](#microsoftappassessment)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppPlatform](#microsoftappplatform)
> - [Microsoft.Attestation](#microsoftattestation)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automanage](#microsoftautomanage)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AVS](#microsoftavs)
> - [Microsoft.Azure.Geneva](#microsoftazuregeneva)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureArcData](#microsoftazurearcdata)
> - [Microsoft.AzureCIS](#microsoftazurecis)
> - [Microsoft.AzureData](#microsoftazuredata)
> - [Microsoft.AzurePercept](#microsoftazurepercept)
> - [Microsoft.AzureSphere](#microsoftazuresphere)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.AzureStackHCI](#microsoftazurestackhci)
> - [Microsoft.BackupSolutions](#microsoftbackupsolutions)
> - [Microsoft.BareMetalInfrastructure](#microsoftbaremetalinfrastructure)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.Billing](#microsoftbilling)
> - [Microsoft.BillingBenefits](#microsoftbillingbenefits)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.BlockchainTokens](#microsoftblockchaintokens)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Capacity](#microsoftcapacity)
> - [Microsoft.Cascade](#microsoftcascade)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ChangeAnalysis](#microsoftchangeanalysis)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicInfrastructureMigrate](#microsoftclassicinfrastructuremigrate)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.ClusterStor](#microsoftclusterstor)
> - [Microsoft.CodeSigning](#microsoftcodesigning)
> - [Microsoft.Codespaces](#microsoftcodespaces)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.Commerce](#microsoftcommerce)
> - [Microsoft.ConfidentialLedger](#microsoftconfidentialledger)
> - [Microsoft.ConnectedCache](#microsoftconnectedcache)
> - [Microsoft.ConnectedVehicle](#microsoftconnectedvehicle)
> - [Microsoft.ConnectedVMwarevSphere](#microsoftconnectedvmwarevsphere)
> - [Microsoft.Consumption](#microsoftconsumption)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerLockbox](#microsoftcustomerlockbox)
> - [Microsoft.CustomProviders](#microsoftcustomproviders)
> - [Microsoft.D365CustomerInsights](#microsoftd365customerinsights)
> - [Microsoft.Dashboard](#microsoftdashboard)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DataProtection](#microsoftdataprotection)
> - [Microsoft.DataShare](#microsoftdatashare)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DelegatedNetwork](#microsoftdelegatednetwork)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.DesktopVirtualization](#microsoftdesktopvirtualization)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DeviceUpdate](#microsoftdeviceupdate)
> - [Microsoft.DevOps](#microsoftdevops)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.Diagnostics](#microsoftdiagnostics)
> - [Microsoft.DigitalTwins](#microsoftdigitaltwins)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.DynamicsLcs](#microsoftdynamicslcs)
> - [Microsoft.EdgeOrder](#microsoftedgeorder)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Experimentation](#microsoftexperimentation)
> - [Microsoft.Falcon](#microsoftfalcon)
> - [Microsoft.Features](#microsoftfeatures)
> - [Microsoft.Fidalgo](#microsoftfidalgo)
> - [Microsoft.FluidRelay](#microsoftfluidrelay)
> - [Microsoft.Gallery](#microsoftgallery)
> - [Microsoft.Genomics](#microsoftgenomics)
> - [Microsoft.Graph](#microsoftgraph)
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HardwareSecurityModules](#microsofthardwaresecuritymodules)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthBot](#microsofthealthbot)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HpcWorkbench](#microsofthpcworkbench)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridConnectivity](#microsofthybridconnectivity)
> - [Microsoft.HybridContainerService](#microsofthybridcontainerservice)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [Microsoft.HybridNetwork](#microsofthybridnetwork)
> - [Microsoft.Hydra](#microsofthydra)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [Microsoft.Intune](#microsoftintune)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSecurity](#microsoftiotsecurity)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kubernetes](#microsoftkubernetes)
> - [Microsoft.KubernetesConfiguration](#microsoftkubernetesconfiguration)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.LocationServices](#microsoftlocationservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.Maintenance](#microsoftmaintenance)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.ManagedServices](#microsoftmanagedservices)
> - [Microsoft.Management](#microsoftmanagement)
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.Marketplace](#microsoftmarketplace)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.MarketplaceNotifications](#microsoftmarketplacenotifications)
> - [Microsoft.MarketplaceOrdering](#microsoftmarketplaceordering)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.MixedReality](#microsoftmixedreality)
> - [Microsoft.MobileNetwork](#microsoftmobilenetwork)
> - [Microsoft.Monitor](#microsoftmonitor)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.NetworkFunction](#microsoftnetworkfunction)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.Notebooks](#microsoftnotebooks)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.ObjectStore](#microsoftobjectstore)
> - [Microsoft.OffAzure](#microsoftoffazure)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.PlayFab](#microsoftplayfab)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.PowerPlatform](#microsoftpowerplatform)
> - [Microsoft.ProjectBabylon](#microsoftprojectbabylon)
> - [Microsoft.ProviderHub](#microsoftproviderhub)
> - [Microsoft.Purview](#microsoftpurview)
> - [Microsoft.Quantum](#microsoftquantum)
> - [Microsoft.Quota](#microsoftquota)
> - [Microsoft.RecommendationsService](#microsoftrecommendationsservice)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.RedHatOpenShift](#microsoftredhatopenshift)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.ResourceConnector](#microsoftresourceconnector)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Scom](#microsoftscom)
> - [Microsoft.ScVmm](#microsoftscvmm)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.SecurityGraph](#microsoftsecuritygraph)
> - [Microsoft.SecurityInsights](#microsoftsecurityinsights)
> - [Microsoft.SerialConsole](#microsoftserialconsole)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.ServiceLinker](#microsoftservicelinker)
> - [Microsoft.Services](#microsoftservices)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.Singularity](#microsoftsingularity)
> - [Microsoft.SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.SQL](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageCache](#microsoftstoragecache)
> - [Microsoft.StorageReplication](#microsoftstoragereplication)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.Subscription](#microsoftsubscription)
> - [Microsoft.Synapse](#microsoftsynapse)
> - [Microsoft.TestBase](#microsofttestbase)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.VideoIndexer](#microsoftvideoindexer)
> - [Microsoft.VirtualMachineImages](#microsoftvirtualmachineimages)
> - [Microsoft.VMware](#microsoftvmware)
> - [Microsoft.VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.VSOnline](#microsoftvsonline)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsDefenderATP](#microsoftwindowsdefenderatp)
> - [Microsoft.WindowsESU](#microsoftwindowsesu)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [Microsoft.WorkloadBuilder](#microsoftworkloadbuilder)
> - [Microsoft.WorkloadMonitor](#microsoftworkloadmonitor)

## Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | DomainServices | Yes |
> | DomainServices / oucontainer | No |

## Microsoft.Addons

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | supportProviders | No |

## Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | aadsupportcases | No |
> | addsservices | No |
> | agents | No |
> | anonymousapiusers | No |
> | configuration | No |
> | logs | No |
> | reports | No |
> | servicehealthmetrics | No |
> | services | No |

## Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | advisorScore | No |
> | configurations | No |
> | generateRecommendations | No |
> | metadata | No |
> | recommendations | No |
> | suppressions | No |

## Microsoft.AgFoodPlatform

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | farmBeats | Yes |
> | farmBeats / eventGridFilters | No |
> | farmBeats / extensions | No |
> | farmBeatsExtensionDefinitions | No |

## Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | actionRules | Yes |
> | alerts | No |
> | alertsList | No |
> | alertsMetaData | No |
> | alertsSummary | No |
> | alertsSummaryList | No |
> | migrateFromSmartDetection | No |
> | resourceHealthAlertRules | Yes |
> | smartDetectorAlertRules | Yes |
> | smartGroups | No |

## Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | servers | Yes |

## Microsoft.AnyBuild

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | clusters | No |

## Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | deletedServices | No |
> | getDomainOwnershipIdentifier | No |
> | reportFeedback | No |
> | service | Yes |
> | service / eventGridFilters | No |
> | validateServiceName | No |

## Microsoft.AppAssessment

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | migrateProjects | No |
> | migrateProjects / assessments | No |
> | migrateProjects / assessments / assessedApplications | No |
> | migrateProjects / assessments / assessedApplications / machines | No |
> | migrateProjects / assessments / assessedMachines | No |
> | migrateProjects / assessments / assessedMachines / applications | No |
> | migrateProjects / assessments / machinesToAssess | No |
> | migrateProjects / sites | No |
> | migrateProjects / sites / applianceConfigurations | No |
> | migrateProjects / sites / machines | No |

## Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | configurationStores | Yes |
> | configurationStores / eventGridFilters | No |
> | configurationStores / keyValues | No |

## Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | Spring | Yes |
> | Spring / apps | No |
> | Spring / apps / deployments | No |

## Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | attestationProviders | Yes |
> | defaultProviders | No |

## Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accessReviewScheduleDefinitions | No |
> | accessReviewScheduleSettings | No |
> | batchResourceCheckAccess | No |
> | classicAdministrators | No |
> | dataAliases | No |
> | dataPolicyManifests | No |
> | denyAssignments | No |
> | diagnosticSettings | No |
> | diagnosticSettingsCategories | No |
> | elevateAccess | No |
> | eligibleChildResources | No |
> | findOrphanRoleAssignments | No |
> | locks | No |
> | permissions | No |
> | policyAssignments | No |
> | policyDefinitions | No |
> | policyExemptions | No |
> | policySetDefinitions | No |
> | privateLinkAssociations | No |
> | providerOperations | No |
> | resourceManagementPrivateLinks | Yes |
> | roleAssignmentApprovals | No |
> | roleAssignments | No |
> | roleAssignmentScheduleInstances | No |
> | roleAssignmentScheduleRequests | No |
> | roleAssignmentSchedules | No |
> | roleAssignmentsUsageMetrics | No |
> | roleDefinitions | No |
> | roleEligibilityScheduleInstances | No |
> | roleEligibilityScheduleRequests | No |
> | roleEligibilitySchedules | No |
> | roleManagementPolicies | No |
> | roleManagementPolicyAssignments | No |

## Microsoft.Automanage

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | Yes |
> | bestPractices | No |
> | bestPractices / versions | No |
> | configurationProfileAssignmentIntents | No |
> | configurationProfileAssignments | No |
> | configurationProfilePreferences | Yes |
> | configurationProfiles | Yes |
> | configurationProfiles / versions | Yes |

## Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | automationAccounts | Yes |
> | automationAccounts / configurations | Yes |
> | automationAccounts / hybridRunbookWorkerGroups | No |
> | automationAccounts / hybridRunbookWorkerGroups / hybridRunbookWorkers | No |
> | automationAccounts / jobs | No |
> | automationAccounts / privateEndpointConnectionProxies | No |
> | automationAccounts / privateEndpointConnections | No |
> | automationAccounts / privateLinkResources | No |
> | automationAccounts / runbooks | Yes |
> | automationAccounts / softwareUpdateConfigurations | No |
> | automationAccounts / webhooks | No |

## Microsoft.AVS

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | privateClouds | Yes |
> | privateClouds / addons | No |
> | privateClouds / authorizations | No |
> | privateClouds / cloudLinks | No |
> | privateClouds / clusters | No |
> | privateClouds / clusters / datastores | No |
> | privateClouds / clusters / placementPolicies | No |
> | privateClouds / clusters / virtualMachines | No |
> | privateClouds / globalReachConnections | No |
> | privateClouds / hcxEnterpriseSites | No |
> | privateClouds / scriptExecutions | No |
> | privateClouds / scriptPackages | No |
> | privateClouds / scriptPackages / scriptCmdlets | No |
> | privateClouds / workloadNetworks | No |
> | privateClouds / workloadNetworks / dhcpConfigurations | No |
> | privateClouds / workloadNetworks / dnsServices | No |
> | privateClouds / workloadNetworks / dnsZones | No |
> | privateClouds / workloadNetworks / gateways | No |
> | privateClouds / workloadNetworks / portMirroringProfiles | No |
> | privateClouds / workloadNetworks / publicIPs | No |
> | privateClouds / workloadNetworks / segments | No |
> | privateClouds / workloadNetworks / virtualMachines | No |
> | privateClouds / workloadNetworks / vmGroups | No |

## Microsoft.Azure.Geneva

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | environments | No |
> | environments / accounts | No |
> | environments / accounts / namespaces | No |
> | environments / accounts / namespaces / configurations | No |

## Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | b2cDirectories | Yes |
> | b2ctenants | No |
> | guestUsages | Yes |

## Microsoft.AzureArcData

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | DataControllers | No |
> | DataWarehouseInstances | No |
> | PostgresInstances | No |
> | SqlManagedInstances | No |
> | SqlServerInstances | No |

## Microsoft.AzureCIS

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | autopilotEnvironments | No |
> | dstsServiceAccounts | No |
> | dstsServiceClientIdentities | No |

## Microsoft.AzureData

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | sqlServerRegistrations | Yes |
> | sqlServerRegistrations / sqlServers | No |

## Microsoft.AzurePercept

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | No |
> | accounts / devices | No |

## Microsoft.AzureSphere

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | catalogs | No |
> | catalogs / certificates | No |
> | catalogs / deployments | No |
> | catalogs / devices | No |
> | catalogs / images | No |
> | catalogs / products | No |
> | catalogs / products / devicegroups | No |

## Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | cloudManifestFiles | No |
> | linkedSubscriptions | Yes |
> | registrations | Yes |
> | registrations / customerSubscriptions | No |
> | registrations / products | No |

## Microsoft.AzureStackHCI

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | clusters | No |
> | clusters / arcsettings | No |
> | clusters / arcsettings / extensions | No |
> | galleryImages | No |
> | networkInterfaces | No |
> | virtualHardDisks | No |
> | virtualmachines | No |
> | virtualmachines / extensions | No |
> | virtualmachines / hybrididentitymetadata | No |
> | virtualNetworks | No |

## Microsoft.BackupSolutions

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | VMwareApplications | Yes |

## Microsoft.BareMetalInfrastructure

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | bareMetalInstances | Yes |

## Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | batchAccounts | Yes |
> | batchAccounts / certificates | No |
> | batchAccounts / pools | No |

## Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | billingAccounts | No |
> | billingAccounts / agreements | No |
> | billingAccounts / appliedReservationOrders | No |
> | billingAccounts / billingPermissions | No |
> | billingAccounts / billingProfiles | No |
> | billingAccounts / billingProfiles / billingPermissions | No |
> | billingAccounts / billingProfiles / billingRoleAssignments | No |
> | billingAccounts / billingProfiles / billingRoleDefinitions | No |
> | billingAccounts / billingProfiles / billingSubscriptions | No |
> | billingAccounts / billingProfiles / createBillingRoleAssignment | No |
> | billingAccounts / billingProfiles / customers | No |
> | billingAccounts / billingProfiles / instructions | No |
> | billingAccounts / billingProfiles / invoices | No |
> | billingAccounts / billingProfiles / invoices / pricesheet | No |
> | billingAccounts / billingProfiles / invoices / transactions | No |
> | billingAccounts / billingProfiles / invoiceSections | No |
> | billingAccounts / billingProfiles / invoiceSections / billingPermissions | No |
> | billingAccounts / billingProfiles / invoiceSections / billingRoleAssignments | No |
> | billingAccounts / billingProfiles / invoiceSections / billingRoleDefinitions | No |
> | billingAccounts / billingProfiles / invoiceSections / billingSubscriptions | No |
> | billingAccounts / billingProfiles / invoiceSections / createBillingRoleAssignment | No |
> | billingAccounts / billingProfiles / invoiceSections / initiateTransfer | No |
> | billingAccounts / billingProfiles / invoiceSections / products | No |
> | billingAccounts / billingProfiles / invoiceSections / products / transfer | No |
> | billingAccounts / billingProfiles / invoiceSections / products / updateAutoRenew | No |
> | billingAccounts / billingProfiles / invoiceSections / transactions | No |
> | billingAccounts / billingProfiles / invoiceSections / transfers | No |
> | billingAccounts / billingProfiles / invoiceSections / validateDeleteInvoiceSectionEligibility | No |
> | billingAccounts / BillingProfiles / patchOperations | No |
> | billingAccounts / billingProfiles / paymentMethodLinks | No |
> | billingAccounts / billingProfiles / paymentMethods | No |
> | billingAccounts / billingProfiles / policies | No |
> | billingAccounts / billingProfiles / pricesheet | No |
> | billingAccounts / billingProfiles / pricesheetDownloadOperations | No |
> | billingAccounts / billingProfiles / products | No |
> | billingAccounts / billingProfiles / reservations | No |
> | billingAccounts / billingProfiles / transactions | No |
> | billingAccounts / billingProfiles / validateDeleteBillingProfileEligibility | No |
> | billingAccounts / billingProfiles / validateDetachPaymentMethodEligibility | No |
> | billingAccounts / billingRoleAssignments | No |
> | billingAccounts / billingRoleDefinitions | No |
> | billingAccounts / billingSubscriptions | No |
> | billingAccounts / billingSubscriptions / elevateRole | No |
> | billingAccounts / billingSubscriptions / invoices | No |
> | billingAccounts / createBillingRoleAssignment | No |
> | billingAccounts / createInvoiceSectionOperations | No |
> | billingAccounts / customers | No |
> | billingAccounts / customers / billingPermissions | No |
> | billingAccounts / customers / billingSubscriptions | No |
> | billingAccounts / customers / initiateTransfer | No |
> | billingAccounts / customers / policies | No |
> | billingAccounts / customers / products | No |
> | billingAccounts / customers / transactions | No |
> | billingAccounts / customers / transfers | No |
> | billingAccounts / customers / transferSupportedAccounts | No |
> | billingAccounts / departments | No |
> | billingAccounts / departments / billingPermissions | No |
> | billingAccounts / departments / billingRoleAssignments | No |
> | billingAccounts / departments / billingRoleDefinitions | No |
> | billingAccounts / departments / billingSubscriptions | No |
> | billingAccounts / departments / enrollmentAccounts | No |
> | billingAccounts / enrollmentAccounts | No |
> | billingAccounts / enrollmentAccounts / billingPermissions | No |
> | billingAccounts / enrollmentAccounts / billingRoleAssignments | No |
> | billingAccounts / enrollmentAccounts / billingRoleDefinitions | No |
> | billingAccounts / enrollmentAccounts / billingSubscriptions | No |
> | billingAccounts / invoices | No |
> | billingAccounts / invoices / transactions | No |
> | billingAccounts / invoices / transactionSummary | No |
> | billingAccounts / invoiceSections | No |
> | billingAccounts / invoiceSections / billingSubscriptionMoveOperations | No |
> | billingAccounts / invoiceSections / billingSubscriptions | No |
> | billingAccounts / invoiceSections / billingSubscriptions / transfer | No |
> | billingAccounts / invoiceSections / elevate | No |
> | billingAccounts / invoiceSections / initiateTransfer | No |
> | billingAccounts / invoiceSections / patchOperations | No |
> | billingAccounts / invoiceSections / productMoveOperations | No |
> | billingAccounts / invoiceSections / products | No |
> | billingAccounts / invoiceSections / products / transfer | No |
> | billingAccounts / invoiceSections / products / updateAutoRenew | No |
> | billingAccounts / invoiceSections / transactions | No |
> | billingAccounts / invoiceSections / transfers | No |
> | billingAccounts / lineOfCredit | No |
> | billingAccounts / patchOperations | No |
> | billingAccounts / payableOverage | No |
> | billingAccounts / paymentMethods | No |
> | billingAccounts / payNow | No |
> | billingAccounts / policies | No |
> | billingAccounts / products | No |
> | billingAccounts / promotionalCredits | No |
> | billingAccounts / reservations | No |
> | billingAccounts / savingsPlanOrders | No |
> | billingAccounts / savingsPlanOrders / savingsPlans | No |
> | billingAccounts / savingsPlans | No |
> | billingAccounts / transactions | No |
> | billingPeriods | No |
> | billingPermissions | No |
> | billingProperty | No |
> | billingRoleAssignments | No |
> | billingRoleDefinitions | No |
> | createBillingRoleAssignment | No |
> | departments | No |
> | enrollmentAccounts | No |
> | invoices | No |
> | paymentMethods | No |
> | promotionalCredits | No |
> | promotions | No |
> | transfers | No |
> | transfers / acceptTransfer | No |
> | transfers / declineTransfer | No |
> | transfers / operationStatus | No |
> | transfers / validateTransfer | No |
> | validateAddress | No |

## Microsoft.BillingBenefits

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | savingsPlanOrderAliases | No |
> | validate | No |

## Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | blockchainMembers | Yes |
> | cordaMembers | Yes |
> | watchers | Yes |

## Microsoft.BlockchainTokens

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | TokenServices | Yes |
> | TokenServices / BlockchainNetworks | No |
> | TokenServices / Groups | No |
> | TokenServices / Groups / Accounts | No |
> | TokenServices / TokenTemplates | No |

## Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | blueprintAssignments | No |
> | blueprintAssignments / assignmentOperations | No |
> | blueprintAssignments / operations | No |
> | blueprints | No |
> | blueprints / artifacts | No |
> | blueprints / versions | No |
> | blueprints / versions / artifacts | No |

## Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | botServices | Yes |
> | botServices / channels | No |
> | botServices / connections | No |
> | botServices / privateEndpointConnectionProxies | No |
> | botServices / privateEndpointConnections | No |
> | botServices / privateLinkResources | No |
> | hostSettings | No |
> | languages | No |
> | templates | No |

## Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | Redis | Yes |
> | Redis / EventGridFilters | No |
> | Redis / privateEndpointConnectionProxies | No |
> | Redis / privateEndpointConnectionProxies / validate | No |
> | Redis / privateEndpointConnections | No |
> | Redis / privateLinkResources | No |
> | redisEnterprise | Yes |
> | redisEnterprise / databases | No |
> | RedisEnterprise / privateEndpointConnectionProxies | No |
> | RedisEnterprise / privateEndpointConnectionProxies / validate | No |
> | RedisEnterprise / privateEndpointConnections | No |
> | RedisEnterprise / privateLinkResources | No |

## Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | appliedReservations | No |
> | autoQuotaIncrease | No |
> | calculateExchange | No |
> | calculatePrice | No |
> | calculatePurchasePrice | No |
> | catalogs | No |
> | commercialReservationOrders | No |
> | exchange | No |
> | ownReservations | No |
> | placePurchaseOrder | No |
> | reservationOrders | No |
> | reservationOrders / calculateRefund | No |
> | reservationOrders / merge | No |
> | reservationOrders / reservations | No |
> | reservationOrders / reservations / revisions | No |
> | reservationOrders / return | No |
> | reservationOrders / split | No |
> | reservationOrders / swap | No |
> | reservations | No |
> | resourceProviders | No |
> | resources | No |
> | validateReservationOrder | No |

## Microsoft.Cascade

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | sites | No |

## Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | No |
> | CdnWebApplicationFirewallPolicies | Yes |
> | edgenodes | No |
> | profiles | Yes |
> | profiles / afdendpoints | Yes |
> | profiles / afdendpoints / routes | No |
> | profiles / customdomains | No |
> | profiles / endpoints | Yes |
> | profiles / endpoints / customdomains | No |
> | profiles / endpoints / origingroups | No |
> | profiles / endpoints / origins | No |
> | profiles / origingroups | No |
> | profiles / origingroups / origins | No |
> | profiles / rulesets | No |
> | profiles / rulesets / rules | No |
> | profiles / secrets | No |
> | profiles / securitypolicies | No |
> | validateProbe | No |

## Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | certificateOrders | Yes |
> | certificateOrders / certificates | No |
> | validateCertificateRegistrationInformation | No |

## Microsoft.ChangeAnalysis

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | changes | No |
> | changeSnapshots | No |
> | computeChanges | No |
> | profile | No |
> | resourceChanges | No |

## Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | capabilities | No |
> | domainNames | Yes |
> | domainNames / capabilities | No |
> | domainNames / internalLoadBalancers | No |
> | domainNames / serviceCertificates | No |
> | domainNames / slots | No |
> | domainNames / slots / roles | No |
> | domainNames / slots / roles / metricDefinitions | No |
> | domainNames / slots / roles / metrics | No |
> | moveSubscriptionResources | No |
> | operatingSystemFamilies | No |
> | operatingSystems | No |
> | quotas | No |
> | resourceTypes | No |
> | validateSubscriptionMoveAvailability | No |
> | virtualMachines | Yes |
> | virtualMachines / diagnosticSettings | No |
> | virtualMachines / metricDefinitions | No |
> | virtualMachines / metrics | No |

## Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | classicInfrastructureResources | No |

## Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | capabilities | No |
> | expressRouteCrossConnections | No |
> | expressRouteCrossConnections / peerings | No |
> | gatewaySupportedDevices | No |
> | networkSecurityGroups | Yes |
> | quotas | No |
> | reservedIps | Yes |
> | virtualNetworks | Yes |
> | virtualNetworks / remoteVirtualNetworkPeeringProxies | No |
> | virtualNetworks / virtualNetworkPeerings | No |

## Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | capabilities | No |
> | disks | No |
> | images | No |
> | osImages | No |
> | osPlatformImages | No |
> | publicImages | No |
> | quotas | No |
> | storageAccounts | Yes |
> | storageAccounts / blobServices | No |
> | storageAccounts / fileServices | No |
> | storageAccounts / metricDefinitions | No |
> | storageAccounts / metrics | No |
> | storageAccounts / queueServices | No |
> | storageAccounts / services | No |
> | storageAccounts / services / diagnosticSettings | No |
> | storageAccounts / services / metricDefinitions | No |
> | storageAccounts / services / metrics | No |
> | storageAccounts / tableServices | No |
> | storageAccounts / vmImages | No |
> | vmImages | No |

## Microsoft.ClusterStor

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | nodes | Yes |

## Microsoft.CodeSigning

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | codeSigningAccounts | No |
> | codeSigningAccounts / certificateProfiles | No |

## Microsoft.Codespaces

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | plans | Yes |
> | registeredSubscriptions | No |

## Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | Yes |
> | accounts / privateEndpointConnectionProxies | No |
> | accounts / privateEndpointConnections | No |
> | accounts / privateLinkResources | No |
> | deletedAccounts | No |

## Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | availabilitySets | Yes |
> | cloudServices | Yes |
> | cloudServices / networkInterfaces | No |
> | cloudServices / publicIPAddresses | No |
> | cloudServices / roleInstances | No |
> | cloudServices / roleInstances / networkInterfaces | No |
> | cloudServices / roles | No |
> | diskAccesses | Yes |
> | diskEncryptionSets | Yes |
> | disks | Yes |
> | galleries | Yes |
> | galleries / applications | No |
> | galleries / applications / versions | No |
> | galleries / images | Yes |
> | galleries / images / versions | Yes |
> | hostGroups | Yes |
> | hostGroups / hosts | Yes |
> | images | Yes |
> | proximityPlacementGroups | Yes |
> | restorePointCollections | Yes |
> | restorePointCollections / restorePoints | No |
> | sharedVMExtensions | Yes |
> | sharedVMExtensions / versions | No |
> | sharedVMImages | Yes |
> | sharedVMImages / versions | No |
> | snapshots | Yes |
> | sshPublicKeys | Yes |
> | virtualMachines | Yes |
> | virtualMachines / extensions | Yes |
> | virtualMachines / metricDefinitions | No |
> | virtualMachines / runCommands | Yes |
> | virtualMachineScaleSets | Yes |
> | virtualMachineScaleSets / extensions | No |
> | virtualMachineScaleSets / networkInterfaces | No |
> | virtualMachineScaleSets / publicIPAddresses | No |
> | virtualMachineScaleSets / virtualMachines | No |
> | virtualMachineScaleSets / virtualMachines / networkInterfaces | No |

## Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | RateCard | No |
> | UsageAggregates | No |

## Microsoft.ConfidentialLedger

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | Ledgers | No |

## Microsoft.ConnectedCache

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | CacheNodes | No |

## Microsoft.ConnectedVehicle

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | platformAccounts | No |
> | registeredSubscriptions | No |

## Microsoft.ConnectedVMwarevSphere

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | Clusters | No |
> | Datastores | No |
> | Hosts | No |
> | ResourcePools | No |
> | VCenters | No |
> | VCenters / InventoryItems | No |
> | VirtualMachines | No |
> | VirtualMachines / Extensions | Yes |
> | VirtualMachines / GuestAgents | No |
> | VirtualMachines / HybridIdentityMetadata | No |
> | VirtualMachineTemplates | No |
> | VirtualNetworks | No |

## Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | AggregatedCost | No |
> | Balances | No |
> | Budgets | No |
> | Charges | No |
> | CostTags | No |
> | credits | No |
> | events | No |
> | Forecasts | No |
> | lots | No |
> | Marketplaces | No |
> | Pricesheets | No |
> | products | No |
> | ReservationDetails | No |
> | ReservationRecommendationDetails | No |
> | ReservationRecommendations | No |
> | ReservationSummaries | No |
> | ReservationTransactions | No |
> | Tags | No |
> | tenants | No |
> | Terms | No |
> | UsageDetails | No |

## Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | containerGroups | Yes |
> | serviceAssociationLinks | No |

## Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | registries | Yes |
> | registries / agentPools | Yes |
> | registries / builds | No |
> | registries / builds / cancel | No |
> | registries / builds / getLogLink | No |
> | registries / buildTasks | Yes |
> | registries / buildTasks / steps | No |
> | registries / connectedRegistries | No |
> | registries / connectedRegistries / deactivate | No |
> | registries / eventGridFilters | No |
> | registries / exportPipelines | No |
> | registries / generateCredentials | No |
> | registries / getBuildSourceUploadUrl | No |
> | registries / GetCredentials | No |
> | registries / importImage | No |
> | registries / importPipelines | No |
> | registries / pipelineRuns | No |
> | registries / privateEndpointConnectionProxies | No |
> | registries / privateEndpointConnectionProxies / validate | No |
> | registries / privateEndpointConnections | No |
> | registries / privateLinkResources | No |
> | registries / queueBuild | No |
> | registries / regenerateCredential | No |
> | registries / regenerateCredentials | No |
> | registries / replications | Yes |
> | registries / runs | No |
> | registries / runs / cancel | No |
> | registries / scheduleRun | No |
> | registries / scopeMaps | No |
> | registries / taskRuns | No |
> | registries / tasks | Yes |
> | registries / tokens | No |
> | registries / updatePolicies | No |
> | registries / webhooks | Yes |
> | registries / webhooks / getCallbackConfig | No |
> | registries / webhooks / ping | No |

## Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | containerServices | Yes |
> | managedClusters | Yes |
> | ManagedClusters / eventGridFilters | No |
> | openShiftManagedClusters | Yes |
> | snapshots | Yes |

## Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | Alerts | No |
> | BenefitUtilizationSummaries | No |
> | BillingAccounts | No |
> | Budgets | No |
> | calculatePrice | No |
> | CloudConnectors | No |
> | Connectors | Yes |
> | costAllocationRules | No |
> | Departments | No |
> | Dimensions | No |
> | EnrollmentAccounts | No |
> | Exports | No |
> | ExternalBillingAccounts | No |
> | ExternalBillingAccounts / Alerts | No |
> | ExternalBillingAccounts / Dimensions | No |
> | ExternalBillingAccounts / Forecast | No |
> | ExternalBillingAccounts / Query | No |
> | ExternalSubscriptions | No |
> | ExternalSubscriptions / Alerts | No |
> | ExternalSubscriptions / Dimensions | No |
> | ExternalSubscriptions / Forecast | No |
> | ExternalSubscriptions / Query | No |
> | fetchPrices | No |
> | Forecast | No |
> | GenerateDetailedCostReport | No |
> | GenerateReservationDetailsReport | No |
> | Insights | No |
> | Query | No |
> | register | No |
> | Reportconfigs | No |
> | Reports | No |
> | ScheduledActions | No |
> | Settings | No |
> | showbackRules | No |
> | Views | No |

## Microsoft.CustomerLockbox

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | DisableLockbox | No |
> | EnableLockbox | No |
> | requests | No |
> | TenantOptedIn | No |

## Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | associations | No |
> | resourceProviders | Yes |

## Microsoft.D365CustomerInsights

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | instances | Yes |

## Microsoft.Dashboard

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | grafana | No |

## Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | jobs | Yes |

## Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | DataBoxEdgeDevices | Yes |

## Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | workspaces | Yes |
> | workspaces / dbWorkspaces | No |
> | workspaces / virtualNetworkPeerings | No |

## Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | catalogs | Yes |

## Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | dataFactories | Yes |
> | dataFactories / diagnosticSettings | No |
> | dataFactories / metricDefinitions | No |
> | dataFactorySchema | No |
> | factories | Yes |
> | factories / integrationRuntimes | No |

## Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | Yes |
> | accounts / dataLakeStoreAccounts | No |
> | accounts / storageAccounts | No |
> | accounts / storageAccounts / containers | No |
> | accounts / transferAnalyticsUnits | No |

## Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | Yes |
> | accounts / eventGridFilters | No |
> | accounts / firewallRules | No |

## Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | DatabaseMigrations | No |
> | services | Yes |
> | services / projects | Yes |
> | SqlMigrationServices | Yes |

## Microsoft.DataProtection

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | BackupVaults | Yes |
> | ResourceGuards | Yes |

## Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | Yes |
> | accounts / shares | No |
> | accounts / shares / datasets | No |
> | accounts / shares / invitations | No |
> | accounts / shares / providersharesubscriptions | No |
> | accounts / shares / synchronizationSettings | No |
> | accounts / sharesubscriptions | No |
> | accounts / sharesubscriptions / consumerSourceDataSets | No |
> | accounts / sharesubscriptions / datasetmappings | No |
> | accounts / sharesubscriptions / triggers | No |

## Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | servers | Yes |
> | servers / advisors | No |
> | servers / keys | No |
> | servers / privateEndpointConnectionProxies | No |
> | servers / privateEndpointConnections | No |
> | servers / privateLinkResources | No |
> | servers / queryTexts | No |
> | servers / recoverableServers | No |
> | servers / resetQueryPerformanceInsightData | No |
> | servers / start | No |
> | servers / stop | No |
> | servers / topQueryStatistics | No |
> | servers / virtualNetworkRules | No |
> | servers / waitStatistics | No |

## Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | flexibleServers | Yes |
> | getPrivateDnsZoneSuffix | No |
> | servers | Yes |
> | servers / advisors | No |
> | servers / keys | No |
> | servers / privateEndpointConnectionProxies | No |
> | servers / privateEndpointConnections | No |
> | servers / privateLinkResources | No |
> | servers / queryTexts | No |
> | servers / recoverableServers | No |
> | servers / resetQueryPerformanceInsightData | No |
> | servers / start | No |
> | servers / stop | No |
> | servers / topQueryStatistics | No |
> | servers / upgrade | No |
> | servers / virtualNetworkRules | No |
> | servers / waitStatistics | No |

## Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | flexibleServers | Yes |
> | getPrivateDnsZoneSuffix | No |
> | serverGroups | Yes |
> | serverGroupsv2 | Yes |
> | servers | Yes |
> | servers / advisors | No |
> | servers / keys | No |
> | servers / privateEndpointConnectionProxies | No |
> | servers / privateEndpointConnections | No |
> | servers / privateLinkResources | No |
> | servers / queryTexts | No |
> | servers / recoverableServers | No |
> | servers / resetQueryPerformanceInsightData | No |
> | servers / topQueryStatistics | No |
> | servers / virtualNetworkRules | No |
> | servers / waitStatistics | No |
> | serversv2 | Yes |

## Microsoft.DelegatedNetwork

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | controller | Yes |
> | delegatedSubnets | Yes |
> | orchestrators | Yes |

## Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | artifactSources | Yes |
> | rollouts | Yes |
> | serviceTopologies | Yes |
> | serviceTopologies / services | Yes |
> | serviceTopologies / services / serviceUnits | Yes |
> | steps | Yes |

## Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | applicationgroups | Yes |
> | applicationgroups / applications | No |
> | applicationgroups / desktops | No |
> | applicationgroups / startmenuitems | No |
> | hostpools | Yes |
> | hostpools / msixpackages | No |
> | hostpools / sessionhosts | No |
> | hostpools / sessionhosts / usersessions | No |
> | hostpools / usersessions | No |
> | scalingplans | Yes |
> | workspaces | Yes |

## Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | ElasticPools | Yes |
> | ElasticPools / IotHubTenants | Yes |
> | ElasticPools / IotHubTenants / securitySettings | No |
> | IotHubs | Yes |
> | IotHubs / eventGridFilters | No |
> | IotHubs / failover | No |
> | IotHubs / securitySettings | No |
> | ProvisioningServices | Yes |
> | usages | No |

## Microsoft.DeviceUpdate

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | No |
> | accounts / instances | No |
> | registeredSubscriptions | No |

## Microsoft.DevOps

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | pipelines | Yes |

## Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | controllers | Yes |

## Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | labcenters | Yes |
> | labs | Yes |
> | labs / environments | Yes |
> | labs / serviceRunners | Yes |
> | labs / virtualMachines | Yes |
> | schedules | Yes |

## Microsoft.Diagnostics

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | AzureKB | No |
> | InsightDiagnostics | No |
> | solutions | No |

## Microsoft.DigitalTwins

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | digitalTwinsInstances | Yes |
> | digitalTwinsInstances / endpoints | No |
> | digitalTwinsInstances / timeSeriesDatabaseConnections | No |

## Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | cassandraClusters | Yes |
> | databaseAccountNames | No |
> | databaseAccounts | Yes |
> | restorableDatabaseAccounts | No |

## Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | domains | Yes |
> | domains / domainOwnershipIdentifiers | No |
> | generateSsoRequest | No |
> | topLevelDomains | No |
> | validateDomainRegistrationInformation | No |

## Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | lcsprojects | No |
> | lcsprojects / clouddeployments | No |
> | lcsprojects / connectors | No |

## Microsoft.EdgeOrder

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | addresses | Yes |
> | orderItems | Yes |
> | orders | No |
> | productFamiliesMetadata | No |

## Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | services | Yes |

## Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | domains | Yes |
> | domains / topics | No |
> | eventSubscriptions | No |
> | extensionTopics | No |
> | partnerNamespaces | Yes |
> | partnerNamespaces / eventChannels | No |
> | partnerRegistrations | Yes |
> | partnerTopics | Yes |
> | partnerTopics / eventSubscriptions | No |
> | systemTopics | Yes |
> | systemTopics / eventSubscriptions | No |
> | topics | Yes |
> | topicTypes | No |

## Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | clusters | Yes |
> | namespaces | Yes |
> | namespaces / authorizationrules | No |
> | namespaces / disasterrecoveryconfigs | No |
> | namespaces / eventhubs | No |
> | namespaces / eventhubs / authorizationrules | No |
> | namespaces / eventhubs / consumergroups | No |
> | namespaces / networkrulesets | No |
> | namespaces / privateEndpointConnections | No |

## Microsoft.Experimentation

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | experimentWorkspaces | Yes |

## Microsoft.Falcon

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | namespaces | Yes |

## Microsoft.Features

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | featureConfigurations | No |
> | featureProviderNamespaces | No |
> | featureProviders | No |
> | features | No |
> | providers | No |
> | subscriptionFeatureRegistrations | No |

## Microsoft.Fidalgo

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | devcenters | No |
> | devcenters / catalogs | No |
> | devcenters / catalogs / items | No |
> | devcenters / environmentTypes | No |
> | devcenters / mappings | No |
> | machinedefinitions | No |
> | networksettings | No |
> | projects | No |
> | projects / catalogItems | No |
> | projects / environments | No |
> | projects / environments / deployments | No |
> | projects / environmentTypes | No |
> | projects / pools | No |

## Microsoft.FluidRelay

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | fluidRelayServers | No |
> | fluidRelayServers / fluidRelayContainers | No |

## Microsoft.Gallery

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | enroll | No |
> | galleryitems | No |
> | generateartifactaccessuri | No |
> | myareas | No |
> | myareas / areas | No |
> | myareas / areas / areas | No |
> | myareas / areas / areas / galleryitems | No |
> | myareas / areas / galleryitems | No |
> | myareas / galleryitems | No |
> | register | No |
> | resources | No |
> | retrieveresourcesbyid | No |

## Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | Yes |

## Microsoft.Graph

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | AzureAdApplication | No |

## Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | autoManagedAccounts | Yes |
> | autoManagedVmConfigurationProfiles | Yes |
> | configurationProfileAssignments | No |
> | guestConfigurationAssignments | No |
> | software | No |
> | softwareUpdateProfile | No |
> | softwareUpdates | No |

## Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | hanaInstances | Yes |
> | sapMonitors | Yes |

## Microsoft.HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | dedicatedHSMs | Yes |

## Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | clusterPools | Yes |
> | clusterPools / clusters | Yes |
> | clusterPools / clusters / instanceViews | No |
> | clusterPools / clusters / serviceConfigs | No |
> | clusterPools / clusters / sessionClusters | Yes |
> | clusterPools / clusters / sessionClusters / instanceViews | No |
> | clusterPools / clusters / sessionClusters / serviceConfigs | No |
> | clusters | Yes |
> | clusters / applications | No |

## Microsoft.HealthBot

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | healthBots | No |

## Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | services | Yes |
> | services / iomtconnectors | No |
> | services / iomtconnectors / connections | No |
> | services / iomtconnectors / mappings | No |
> | services / privateEndpointConnectionProxies | No |
> | services / privateEndpointConnections | No |
> | services / privateLinkResources | No |
> | workspaces | Yes |
> | workspaces / dicomservices | Yes |
> | workspaces / eventGridFilters | No |
> | workspaces / fhirservices | Yes |
> | workspaces / iotconnectors | Yes |
> | workspaces / iotconnectors / destinations | No |
> | workspaces / iotconnectors / fhirdestinations | No |
> | workspaces / privateEndpointConnectionProxies | No |
> | workspaces / privateEndpointConnections | No |
> | workspaces / privateLinkResources | No |

## Microsoft.HpcWorkbench

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | instances | No |

## Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | machines | Yes |
> | machines / assessPatches | No |
> | machines / extensions | Yes |
> | machines / installPatches | No |
> | machines / privateLinkScopes | No |
> | privateLinkScopes | Yes |
> | privateLinkScopes / privateEndpointConnectionProxies | No |
> | privateLinkScopes / privateEndpointConnections | No |

## Microsoft.HybridConnectivity

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | endpoints | No |

## Microsoft.HybridContainerService

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | provisionedClusters | No |

## Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | dataManagers | Yes |

## Microsoft.HybridNetwork

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | devices | No |
> | networkFunctions | No |
> | networkFunctionVendors | No |
> | registeredSubscriptions | No |
> | vendors | No |
> | vendors / vendorSkus | No |
> | vendors / vendorskus / previewSubscriptions | No |

## Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | components | Yes |
> | networkScopes | Yes |

## Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | jobs | Yes |

## Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | diagnosticSettings | No |
> | diagnosticSettingsCategories | No |

## Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | appTemplates | No |
> | IoTApps | Yes |

## Microsoft.IoTSecurity

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | alertTypes | No |
> | defenderSettings | No |
> | onPremiseSensors | No |
> | recommendationTypes | No |
> | sensors | No |
> | sites | No |

## Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | Graph | Yes |

## Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | deletedManagedHSMs | No |
> | deletedVaults | No |
> | hsmPools | Yes |
> | managedHSMs | Yes |
> | vaults | Yes |
> | vaults / accessPolicies | No |
> | vaults / eventGridFilters | No |
> | vaults / keys | No |
> | vaults / keys / versions | No |
> | vaults / secrets | No |

## Microsoft.Kubernetes

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | connectedClusters | No |
> | registeredSubscriptions | No |

## Microsoft.KubernetesConfiguration

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | extensions | No |
> | fluxConfigurations | No |
> | sourceControlConfigurations | No |

## Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | clusters | Yes |
> | clusters / attacheddatabaseconfigurations | No |
> | clusters / databases | No |
> | clusters / databases / dataconnections | No |
> | clusters / databases / eventhubconnections | No |
> | clusters / databases / principalassignments | No |
> | clusters / databases / scripts | No |
> | clusters / dataconnections | No |
> | clusters / principalassignments | No |
> | clusters / sharedidentities | No |

## Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | labaccounts | Yes |
> | labplans | Yes |
> | labs | Yes |
> | users | No |

## Microsoft.LocationServices

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | Yes |

## Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | hostingEnvironments | Yes |
> | integrationAccounts | Yes |
> | integrationServiceEnvironments | Yes |
> | integrationServiceEnvironments / managedApis | Yes |
> | isolatedEnvironments | Yes |
> | workflows | Yes |

## Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | commitmentPlans | Yes |
> | webServices | Yes |
> | Workspaces | Yes |

## Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | aisysteminventories | Yes |
> | virtualclusters | Yes |
> | workspaces | Yes |
> | workspaces / batchEndpoints | Yes |
> | workspaces / batchEndpoints / deployments | Yes |
> | workspaces / batchEndpoints / deployments / jobs | No |
> | workspaces / batchEndpoints / jobs | No |
> | workspaces / codes | No |
> | workspaces / codes / versions | No |
> | workspaces / components | No |
> | workspaces / components / versions | No |
> | workspaces / computes | No |
> | workspaces / data | No |
> | workspaces / datasets | No |
> | workspaces / datastores | No |
> | workspaces / environments | No |
> | workspaces / eventGridFilters | No |
> | workspaces / jobs | No |
> | workspaces / labelingJobs | No |
> | workspaces / linkedServices | No |
> | workspaces / models | No |
> | workspaces / models / versions | No |
> | workspaces / onlineEndpoints | Yes |
> | workspaces / onlineEndpoints / deployments | Yes |

## Microsoft.Maintenance

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | applyUpdates | No |
> | configurationAssignments | No |
> | maintenanceConfigurations | Yes |
> | publicMaintenanceConfigurations | No |
> | updates | No |

## Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | Identities | No |
> | userAssignedIdentities | Yes |

## Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | marketplaceRegistrationDefinitions | No |
> | registrationAssignments | No |
> | registrationDefinitions | No |

## Microsoft.Management

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | getEntities | No |
> | managementGroups | No |
> | managementGroups / settings | No |
> | resources | No |
> | startTenantBackfill | No |
> | tenantBackfillStatus | No |

## Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | Yes |
> | accounts / creators | Yes |
> | accounts / eventGridFilters | No |
> | accounts / privateAtlases | Yes |

## Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | macc | No |
> | offers | No |
> | offerTypes | No |
> | offerTypes / publishers | No |
> | offerTypes / publishers / offers | No |
> | offerTypes / publishers / offers / plans | No |
> | offerTypes / publishers / offers / plans / agreements | No |
> | offerTypes / publishers / offers / plans / configs | No |
> | offerTypes / publishers / offers / plans / configs / importImage | No |
> | privategalleryitems | No |
> | privateStoreClient | No |
> | privateStores | No |
> | privateStores / AdminRequestApprovals | No |
> | privateStores / billingAccounts | No |
> | privateStores / bulkCollectionsAction | No |
> | privateStores / collections | No |
> | privateStores / collections / offers | No |
> | privateStores / collections / transferOffers | No |
> | privateStores / collectionsToSubscriptionsMapping | No |
> | privateStores / offers | No |
> | privateStores / offers / acknowledgeNotification | No |
> | privateStores / queryApprovedPlans | No |
> | privateStores / queryNotificationsState | No |
> | privateStores / queryOffers | No |
> | privateStores / RequestApprovals | No |
> | privateStores / requestApprovals / query | No |
> | privateStores / requestApprovals / withdrawPlan | No |
> | products | No |
> | publishers | No |
> | publishers / offers | No |
> | publishers / offers / amendments | No |
> | register | No |

## Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | classicDevServices | Yes |
> | updateCommunicationPreference | No |

## Microsoft.MarketplaceNotifications

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | reviewsnotifications | No |

## Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | agreements | No |
> | offertypes | No |

## Microsoft.Media

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | mediaservices | Yes |
> | mediaservices / accountFilters | No |
> | mediaservices / assets | No |
> | mediaservices / assets / assetFilters | No |
> | mediaservices / contentKeyPolicies | No |
> | mediaservices / eventGridFilters | No |
> | mediaservices / graphInstances | No |
> | mediaservices / graphTopologies | No |
> | mediaservices / liveEventOperations | No |
> | mediaservices / liveEvents | Yes |
> | mediaservices / liveEvents / liveOutputs | No |
> | mediaservices / liveOutputOperations | No |
> | mediaservices / mediaGraphs | No |
> | mediaservices / privateEndpointConnectionOperations | No |
> | mediaservices / privateEndpointConnectionProxies | No |
> | mediaservices / privateEndpointConnections | No |
> | mediaservices / streamingEndpointOperations | No |
> | mediaservices / streamingEndpoints | Yes |
> | mediaservices / streamingLocators | No |
> | mediaservices / streamingPolicies | No |
> | mediaservices / transforms | No |
> | mediaservices / transforms / jobs | No |
> | videoAnalyzers | Yes |
> | videoAnalyzers / accessPolicies | No |
> | videoAnalyzers / edgeModules | No |
> | videoAnalyzers / livePipelines | No |
> | videoAnalyzers / pipelineJobs | No |
> | videoAnalyzers / pipelineTopologies | No |
> | videoAnalyzers / videos | No |

## Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | assessmentProjects | Yes |
> | migrateprojects | Yes |
> | moveCollections | Yes |
> | projects | Yes |

## Microsoft.MixedReality

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | holographicsBroadcastAccounts | Yes |
> | objectAnchorsAccounts | Yes |
> | objectUnderstandingAccounts | Yes |
> | remoteRenderingAccounts | Yes |
> | spatialAnchorsAccounts | Yes |

## Microsoft.MobileNetwork

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | mobileNetworks | No |
> | mobileNetworks / dataNetworks | No |
> | mobileNetworks / services | No |
> | mobileNetworks / simPolicies | No |
> | mobileNetworks / sites | No |
> | mobileNetworks / slices | No |
> | networks | No |
> | networks / sites | No |
> | packetCoreControlPlanes | No |
> | packetCoreControlPlanes / packetCoreDataPlanes | No |
> | packetCoreControlPlanes / packetCoreDataPlanes / attachedDataNetworks | No |
> | packetCores | No |
> | sims | No |
> | sims / simProfiles | No |

## Microsoft.Monitor

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | Yes |

## Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | netAppAccounts | Yes |
> | netAppAccounts / accountBackups | No |
> | netAppAccounts / capacityPools | Yes |
> | netAppAccounts / capacityPools / volumes | Yes |
> | netAppAccounts / capacityPools / volumes / snapshots | No |
> | netAppAccounts / capacityPools / volumes / subvolumes | No |
> | netAppAccounts / snapshotPolicies | Yes |
> | netAppAccounts / volumeGroups | No |

## Microsoft.NetworkFunction

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | azureTrafficCollectors | Yes |
## Microsoft.Network

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | applicationGateways | Yes |
> | applicationGatewayWebApplicationFirewallPolicies | Yes |
> | applicationSecurityGroups | Yes |
> | azureFirewallFqdnTags | No |
> | azureFirewalls | Yes |
> | bastionHosts | Yes |
> | bgpServiceCommunities | No |
> | connections | Yes |
> | ddosCustomPolicies | Yes |
> | ddosProtectionPlans | Yes |
> | dnsOperationStatuses | No |
> | dnszones | Yes |
> | dnszones / A | No |
> | dnszones / AAAA | No |
> | dnszones / all | No |
> | dnszones / CAA | No |
> | dnszones / CNAME | No |
> | dnszones / MX | No |
> | dnszones / NS | No |
> | dnszones / PTR | No |
> | dnszones / recordsets | No |
> | dnszones / SOA | No |
> | dnszones / SRV | No |
> | dnszones / TXT | No |
> | expressRouteCircuits | Yes |
> | expressRouteCrossConnections | Yes |
> | expressRouteGateways | Yes |
> | expressRoutePorts | Yes |
> | expressRouteServiceProviders | No |
> | firewallPolicies | Yes |
> | frontdoors | Yes |
> | frontdoorWebApplicationFirewallManagedRuleSets | No |
> | frontdoorWebApplicationFirewallPolicies | Yes |
> | getDnsResourceReference | No |
> | internalNotify | No |
> | ipGroups | Yes |
> | loadBalancers | Yes |
> | localNetworkGateways | Yes |
> | natGateways | Yes |
> | networkIntentPolicies | Yes |
> | networkInterfaces | Yes |
> | networkProfiles | Yes |
> | networkSecurityGroups | Yes |
> | networkWatchers | Yes |
> | networkWatchers / connectionMonitors | Yes |
> | networkWatchers / flowLogs | Yes |
> | networkWatchers / lenses | Yes |
> | networkWatchers / pingMeshes | Yes |
> | p2sVpnGateways | Yes |
> | privateDnsOperationStatuses | No |
> | privateDnsZones | Yes |
> | privateDnsZones / A | No |
> | privateDnsZones / AAAA | No |
> | privateDnsZones / all | No |
> | privateDnsZones / CNAME | No |
> | privateDnsZones / MX | No |
> | privateDnsZones / PTR | No |
> | privateDnsZones / SOA | No |
> | privateDnsZones / SRV | No |
> | privateDnsZones / TXT | No |
> | privateDnsZones / virtualNetworkLinks | Yes |
> | privateEndpoints | Yes |
> | privateLinkServices | Yes |
> | publicIPAddresses | Yes |
> | publicIPPrefixes | Yes |
> | routeFilters | Yes |
> | routeTables | Yes |
> | serviceEndpointPolicies | Yes |
> | trafficManagerGeographicHierarchies | No |
> | trafficmanagerprofiles | Yes |
> | trafficmanagerprofiles / heatMaps | No |
> | trafficManagerUserMetricsKeys | No |
> | virtualHubs | Yes |
> | virtualNetworkGateways | Yes |
> | virtualNetworks | Yes |
> | virtualNetworks / subnets | No |
> | virtualNetworkTaps | Yes |
> | virtualWans | Yes |
> | vpnGateways | Yes |
> | vpnSites | Yes |
> | webApplicationFirewallPolicies | Yes |

## Microsoft.Notebooks

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | NotebookProxies | No |

## Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | namespaces | Yes |
> | namespaces / notificationHubs | Yes |

## Microsoft.ObjectStore

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | osNamespaces | No |

## Microsoft.OffAzure

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | HyperVSites | Yes |
> | ImportSites | Yes |
> | MasterSites | Yes |
> | ServerSites | Yes |
> | VMwareSites | Yes |

## Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | clusters | Yes |
> | deletedWorkspaces | No |
> | linkTargets | No |
> | querypacks | Yes |
> | storageInsightConfigs | No |
> | workspaces | Yes |
> | workspaces / dataExports | No |
> | workspaces / dataSources | No |
> | workspaces / linkedServices | No |
> | workspaces / linkedStorageAccounts | No |
> | workspaces / metadata | No |
> | workspaces / query | No |
> | workspaces / scopedPrivateLinkProxies | No |
> | workspaces / storageInsightConfigs | No |
> | workspaces / tables | No |

## Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | managementassociations | No |
> | managementconfigurations | Yes |
> | solutions | Yes |
> | views | Yes |

## Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | cdnPeeringPrefixes | No |
> | legacyPeerings | No |
> | lookingGlass | No |
> | peerAsns | No |
> | peerings | Yes |
> | peeringServiceCountries | No |
> | peeringServiceProviders | No |
> | peeringServices | Yes |

## Microsoft.PlayFab

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | PlayerAccountPools | No |
> | Titles | No |

## Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | attestations | No |
> | eventGridFilters | No |
> | policyEvents | No |
> | policyMetadata | No |
> | policyStates | No |
> | policyTrackedResources | No |
> | remediations | No |

## Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | consoles | No |
> | dashboards | Yes |
> | tenantconfigurations | No |
> | userSettings | No |

## Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | privateLinkServicesForPowerBI | Yes |
> | tenants | Yes |
> | tenants / workspaces | No |
> | workspaceCollections | Yes |

## Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | autoScaleVCores | Yes |
> | capacities | Yes |
> | servers | Yes |

## Microsoft.PowerPlatform

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | Yes |
> | enterprisePolicies | Yes |

## Microsoft.ProjectBabylon

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | Yes |
> | deletedAccounts | No |

## Microsoft.ProviderHub

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | providerRegistrations | No |
> | providerRegistrations / customRollouts | No |
> | providerRegistrations / defaultRollouts | No |
> | providerRegistrations / resourceActions | No |
> | providerRegistrations / resourceTypeRegistrations | No |

## Microsoft.Purview

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | Yes |
> | deletedAccounts | No |
> | getDefaultAccount | No |
> | removeDefaultAccount | No |
> | setDefaultAccount | No |

## Microsoft.Quantum

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | Workspaces | No |

## Microsoft.Quota

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | quotaRequests | No |
> | quotas | No |
> | usages | No |

## Microsoft.RecommendationsService

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | No |
> | accounts / modeling | No |
> | accounts / serviceEndpoints | No |

## Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | backupProtectedItems | No |
> | vaults | Yes |

## Microsoft.RedHatOpenShift

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | OpenShiftClusters | Yes |

## Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | namespaces | Yes |
> | namespaces / authorizationrules | No |
> | namespaces / hybridconnections | No |
> | namespaces / hybridconnections / authorizationrules | No |
> | namespaces / privateEndpointConnections | No |
> | namespaces / wcfrelays | No |
> | namespaces / wcfrelays / authorizationrules | No |

## Microsoft.ResourceConnector

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | appliances | Yes |

## Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | queries | Yes |
> | resourceChangeDetails | No |
> | resourceChanges | No |
> | resources | No |
> | resourcesHistory | No |
> | subscriptionsStatus | No |

## Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | availabilityStatuses | No |
> | childAvailabilityStatuses | No |
> | childResources | No |
> | emergingissues | No |
> | events | No |
> | impactedResources | No |
> | metadata | No |

## Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | deployments | No |
> | deployments / operations | No |
> | deploymentScripts | Yes |
> | deploymentScripts / logs | No |
> | deploymentStacks | No |
> | deploymentStacks / snapshots | No |
> | links | No |
> | providers | No |
> | resourceGroups | No |
> | subscriptions | No |
> | templateSpecs | Yes |
> | templateSpecs / versions | Yes |
> | tenants | No |

## Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | applications | Yes |
> | resources | Yes |
> | saasresources | No |

## Microsoft.Scom

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | managedInstances | No |

## Microsoft.ScVmm

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | clouds | No |
> | VirtualMachines | No |
> | VirtualMachineTemplates | No |
> | VirtualNetworks | No |
> | vmmservers | No |

## Microsoft.Search

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | resourceHealthMetadata | No |
> | searchServices | Yes |

## Microsoft.Security

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | adaptiveNetworkHardenings | No |
> | advancedThreatProtectionSettings | No |
> | alerts | No |
> | alertsSuppressionRules | No |
> | allowedConnections | No |
> | antiMalwareSettings | No |
> | applicationWhitelistings | No |
> | assessmentMetadata | No |
> | assessments | No |
> | assignments | Yes |
> | autoDismissAlertsRules | No |
> | automations | Yes |
> | AutoProvisioningSettings | No |
> | Compliances | No |
> | connectedContainerRegistries | No |
> | connectors | No |
> | customAssessmentAutomations | Yes |
> | customEntityStoreAssignments | Yes |
> | dataCollectionAgents | No |
> | deviceSecurityGroups | No |
> | discoveredSecuritySolutions | No |
> | externalSecuritySolutions | No |
> | InformationProtectionPolicies | No |
> | ingestionSettings | No |
> | insights | No |
> | iotSecuritySolutions | Yes |
> | iotSecuritySolutions / analyticsModels | No |
> | iotSecuritySolutions / analyticsModels / aggregatedAlerts | No |
> | iotSecuritySolutions / analyticsModels / aggregatedRecommendations | No |
> | iotSecuritySolutions / iotAlerts | No |
> | iotSecuritySolutions / iotAlertTypes | No |
> | iotSecuritySolutions / iotRecommendations | No |
> | iotSecuritySolutions / iotRecommendationTypes | No |
> | jitNetworkAccessPolicies | No |
> | jitPolicies | No |
> | policies | No |
> | pricings | No |
> | regulatoryComplianceStandards | No |
> | regulatoryComplianceStandards / regulatoryComplianceControls | No |
> | regulatoryComplianceStandards / regulatoryComplianceControls / regulatoryComplianceAssessments | No |
> | secureScoreControlDefinitions | No |
> | secureScoreControls | No |
> | secureScores | No |
> | secureScores / secureScoreControls | No |
> | securityConnectors | Yes |
> | securityContacts | No |
> | securitySolutions | No |
> | securitySolutionsReferenceData | No |
> | securityStatuses | No |
> | securityStatusesSummaries | No |
> | serverVulnerabilityAssessments | No |
> | settings | No |
> | sqlVulnerabilityAssessments | No |
> | standards | Yes |
> | subAssessments | No |
> | tasks | No |
> | topologies | No |
> | workspaceSettings | No |

## Microsoft.SecurityGraph

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | diagnosticSettings | No |
> | diagnosticSettingsCategories | No |

## Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | aggregations | No |
> | alertRules | No |
> | alertRuleTemplates | No |
> | automationRules | No |
> | bookmarks | No |
> | cases | No |
> | dataConnectors | No |
> | dataConnectorsCheckRequirements | No |
> | enrichment | No |
> | entities | No |
> | entityQueries | No |
> | entityQueryTemplates | No |
> | incidents | No |
> | metadata | No |
> | officeConsents | No |
> | onboardingStates | No |
> | settings | No |
> | sourceControls | No |
> | threatIntelligence | No |
> | watchlists | No |

## Microsoft.SerialConsole

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | consoleServices | No |
> | serialPorts | No |

## Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | namespaces | Yes |
> | namespaces / authorizationrules | No |
> | namespaces / disasterrecoveryconfigs | No |
> | namespaces / eventgridfilters | No |
> | namespaces / networkrulesets | No |
> | namespaces / privateEndpointConnections | No |
> | namespaces / queues | No |
> | namespaces / queues / authorizationrules | No |
> | namespaces / topics | No |
> | namespaces / topics / authorizationrules | No |
> | namespaces / topics / subscriptions | No |
> | namespaces / topics / subscriptions / rules | No |
> | premiumMessagingRegions | No |

## Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | applications | Yes |
> | clusters | Yes |
> | clusters / applications | No |
> | containerGroups | Yes |
> | containerGroupSets | Yes |
> | edgeclusters | Yes |
> | edgeclusters / applications | No |
> | managedclusters | Yes |
> | managedclusters / applications | No |
> | managedclusters / applications / services | No |
> | managedclusters / applicationTypes | No |
> | managedclusters / applicationTypes / versions | No |
> | managedclusters / nodetypes | No |
> | networks | Yes |
> | secretstores | Yes |
> | secretstores / certificates | No |
> | secretstores / secrets | No |
> | volumes | Yes |

## Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | applications | Yes |
> | containerGroups | Yes |
> | gateways | Yes |
> | networks | Yes |
> | secrets | Yes |
> | volumes | Yes |

## Microsoft.ServiceLinker

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | linkers | No |

## Microsoft.Services

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | providerRegistrations | No |
> | providerRegistrations / resourceTypeRegistrations | No |
> | rollouts | Yes |

## Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | SignalR | Yes |
> | SignalR / eventGridFilters | No |
> | WebPubSub | Yes |

## Microsoft.Singularity

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | Yes |
> | accounts / accountQuotaPolicies | No |
> | accounts / groupPolicies | No |
> | accounts / jobs | No |
> | accounts / models | No |
> | accounts / storageContainers | No |
> | images | No |
> | quotas | No |

## Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | hybridUseBenefits | No |

## Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | applicationDefinitions | Yes |
> | applications | Yes |
> | jitRequests | Yes |

## Microsoft.SQL

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | managedInstances | Yes |
> | managedInstances / databases | Yes |
> | managedInstances / databases / backupShortTermRetentionPolicies | No |
> | managedInstances / databases / schemas / tables / columns / sensitivityLabels | No |
> | managedInstances / databases / vulnerabilityAssessments | No |
> | managedInstances / databases / vulnerabilityAssessments / rules / baselines | No |
> | managedInstances / encryptionProtector | No |
> | managedInstances / keys | No |
> | managedInstances / restorableDroppedDatabases / backupShortTermRetentionPolicies | No |
> | managedInstances / vulnerabilityAssessments | No |
> | servers | Yes |
> | servers / administrators | No |
> | servers / communicationLinks | No |
> | servers / databases | Yes |
> | servers / encryptionProtector | No |
> | servers / firewallRules | No |
> | servers / keys | No |
> | servers / restorableDroppedDatabases | No |
> | servers / serviceobjectives | No |
> | servers / tdeCertificates | No |
> | virtualClusters | No |

## Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | SqlVirtualMachineGroups | Yes |
> | SqlVirtualMachineGroups / AvailabilityGroupListeners | No |
> | SqlVirtualMachines | Yes |

## Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | deletedAccounts | No |
> | storageAccounts | Yes |
> | storageAccounts / blobServices | No |
> | storageAccounts / encryptionScopes | No |
> | storageAccounts / fileServices | No |
> | storageAccounts / queueServices | No |
> | storageAccounts / services | No |
> | storageAccounts / services / metricDefinitions | No |
> | storageAccounts / tableServices | No |
> | usages | No |

## Microsoft.StorageCache

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | amlFilesystems | Yes |
> | caches | Yes |
> | caches / storageTargets | No |
> | usageModels | No |

## Microsoft.StorageReplication

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | replicationGroups | No |

## Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | storageSyncServices | Yes |
> | storageSyncServices / registeredServers | No |
> | storageSyncServices / syncGroups | No |
> | storageSyncServices / syncGroups / cloudEndpoints | No |
> | storageSyncServices / syncGroups / serverEndpoints | No |
> | storageSyncServices / workflows | No |

## Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | managers | Yes |

## Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | clusters | Yes |
> | clusters / privateEndpoints | No |
> | streamingjobs | Yes |

## Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | acceptChangeTenant | No |
> | acceptOwnership | No |
> | acceptOwnershipStatus | No |
> | aliases | No |
> | cancel | No |
> | changeTenantRequest | No |
> | changeTenantStatus | No |
> | CreateSubscription | No |
> | enable | No |
> | policies | No |
> | rename | No |
> | SubscriptionDefinitions | No |
> | SubscriptionOperations | No |
> | subscriptions | No |

## Microsoft.Synapse

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | kustoOperations | No |
> | privateLinkHubs | Yes |
> | workspaces | Yes |
> | workspaces / bigDataPools | Yes |
> | workspaces / kustoPools | Yes |
> | workspaces / kustoPools / attacheddatabaseconfigurations | No |
> | workspaces / kustoPools / databases | No |
> | workspaces / kustoPools / databases / dataconnections | No |
> | workspaces / operationStatuses | No |
> | workspaces / sqlDatabases | Yes |
> | workspaces / sqlPools | Yes |

## Microsoft.TestBase

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | testBaseAccounts | No |
> | testBaseAccounts / customerEvents | No |
> | testBaseAccounts / emailEvents | No |
> | testBaseAccounts / flightingRings | No |
> | testBaseAccounts / packages | No |
> | testBaseAccounts / packages / favoriteProcesses | No |
> | testBaseAccounts / packages / osUpdates | No |
> | testBaseAccounts / testSummaries | No |
> | testBaseAccounts / testTypes | No |
> | testBaseAccounts / usages | No |

## Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | environments | Yes |
> | environments / accessPolicies | No |
> | environments / eventsources | Yes |
> | environments / privateEndpointConnectionProxies | No |
> | environments / privateEndpointConnections | No |
> | environments / privateLinkResources | No |
> | environments / referenceDataSets | Yes |

## Microsoft.VideoIndexer

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | No |

## Microsoft.VirtualMachineImages

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | imageTemplates | Yes |
> | imageTemplates / runOutputs | No |

## Microsoft.VMware

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | arczones | No |
> | resourcepools | No |
> | vcenters | No |
> | VCenters / InventoryItems | No |
> | virtualmachines | No |
> | virtualmachinetemplates | No |
> | virtualnetworks | No |

## Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | dedicatedCloudNodes | Yes |
> | dedicatedCloudServices | Yes |
> | virtualMachines | Yes |

## Microsoft.VSOnline

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | accounts | Yes |
> | plans | Yes |
> | registeredSubscriptions | No |
## Microsoft.Web

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | apiManagementAccounts | No |
> | apiManagementAccounts / apiAcls | No |
> | apiManagementAccounts / apis | No |
> | apiManagementAccounts / apis / apiAcls | No |
> | apiManagementAccounts / apis / connectionAcls | No |
> | apiManagementAccounts / apis / connections | No |
> | apiManagementAccounts / apis / connections / connectionAcls | No |
> | apiManagementAccounts / apis / localizedDefinitions | No |
> | apiManagementAccounts / connectionAcls | No |
> | apiManagementAccounts / connections | No |
> | billingMeters | No |
> | certificates | Yes |
> | connectionGateways | Yes |
> | connections | Yes |
> | customApis | Yes |
> | deletedSites | No |
> | functionAppStacks | No |
> | generateGithubAccessTokenForAppserviceCLI | No |
> | hostingEnvironments | Yes |
> | hostingEnvironments / eventGridFilters | No |
> | hostingEnvironments / multiRolePools | No |
> | hostingEnvironments / workerPools | No |
> | kubeEnvironments | Yes |
> | publishingUsers | No |
> | recommendations | No |
> | resourceHealthMetadata | No |
> | runtimes | No |
> | serverFarms | Yes |
> | serverFarms / eventGridFilters | No |
> | serverFarms / firstPartyApps | No |
> | serverFarms / firstPartyApps / keyVaultSettings | No |
> | sites | Yes |
> | sites/config  | No |
> | sites / eventGridFilters | No |
> | sites / hostNameBindings | No |
> | sites / networkConfig | No |
> | sites / premieraddons | Yes |
> | sites / slots | Yes |
> | sites / slots / eventGridFilters | No |
> | sites / slots / hostNameBindings | No |
> | sites / slots / networkConfig | No |
> | sourceControls | No |
> | staticSites | Yes |
> | validate | No |
> | verifyHostingEnvironmentVnet | No |
> | webAppStacks | No |

## Microsoft.WindowsDefenderATP

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | diagnosticSettings | No |
> | diagnosticSettingsCategories | No |

## Microsoft.WindowsESU

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | multipleActivationKeys | Yes |

## Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | DeviceServices | Yes |

## Microsoft.WorkloadBuilder

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | migrationAgents | No |
> | workloads | No |
> | workloads / instances | No |
> | workloads / versions | No |
> | workloads / versions / artifacts | No |

## Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Resource type | Complete mode deletion |
> | ------------- | ----------- |
> | monitors | No |

## Next steps

To get the same data as a file of comma-separated values, download [complete-mode-deletion.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv).