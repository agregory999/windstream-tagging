# Windstream Tagging

Using bulk tagging to define which resources to tag and to what.  Implies that tag namespaces are created ahead of time.

## API Call

Use the bulk-tag operation from the CLI, the easiest way is a JSON file.  Here is an example input.  Note that comaprtment is not specified here, as these resources are in different compartments:

```json
{
  "bulkEditOperations": [
    {
      "definedTags": {
        "CIS235-namesp": {
          "CreatedBy": "me!!"
        }
      },
      "operationType": "ADD_OR_SET"
    }
  ],
  "maxWaitSeconds": 0,
  "resources": [
    {
      "id": "ocid1.instance.oc1.phx.anyhqljtytsgwaycbkw5karymvcguqxbhx3buztcjjl33vberu62f5ysadca",
      "resourceType": "Instance"
    },
    {
      "id": "ocid1.bootvolume.oc1.phx.abyhqljtjmpf55eh57z73fj7hfgcox3fohw6yaj3qggryamf23dwqkaagr4a",
      "resourceType": "BootVolume"
    },
    {
      "id": "ocid1.vcn.oc1.phx.amaaaaaaytsgwayay4blhynemcmccg7z7m53pkkqojw6dg3zdzqidytgo62a",
      "resourceType": "Vcn"
    }
  ],
  "waitForState": [
    "IN_PROGRESS"
  ],
  "waitIntervalSeconds": 0
}
```

## Actual API call 

Example of the CLI call itself.  Note that the call returns before the operation succeeds, but we can uopdate the "waitForState" above to make it wait for compeltion.   Work is done async, thus the work request that is returned.

```bash
windstream-tagging % oci iam tag bulk-edit --from-json file://tag-update-cis.json
Action completed. Waiting until the work request has entered state: ('IN_PROGRESS',)
Failed to wait until the work request entered the specified state. Outputting last known resource state
{
  "opc-work-request-id": "ocid1.taggingworkrequest.oc1..aaaaaaaaprvzc322lhakcpuxx4towaht6wn7p7adrxwb45rxsvb74mxwosxa"
}
```

Once the script completes, we can use the request ID to generate a report:

```
windstream-tagging % oci iam tagging-work-request get --work-request-id ocid1.taggingworkrequest.oc1..aaaaaaaaq4ocs4hrazmmyclbigf4pkardhiuq3ve6jqf6q4pnbab7e6aaqna
{
  "data": {
    "compartment-id": "ocid1.compartment.oc1..aaaaaaaaxv3eh5mse7dtef4o7sllbcnx6df2rvnsb6gbedpviqt2phbnfzfq",
    "id": "ocid1.taggingworkrequest.oc1..aaaaaaaaq4ocs4hrazmmyclbigf4pkardhiuq3ve6jqf6q4pnbab7e6aaqna",
    "operation-type": "BULK_EDIT_OF_TAGS",
    "percent-complete": 100.0,
    "resources": [
      {
        "action-type": "UPDATED",
        "entity-type": "vcns",
        "entity-uri": "https://iaas.us-phoenix-1.oraclecloud.com/20160918/vcns/ocid1.vcn.oc1.phx.amaaaaaaytsgwayay4blhynemcmccg7z7m53pkkqojw6dg3zdzqidytgo62a",
        "identifier": "ocid1.vcn.oc1.phx.amaaaaaaytsgwayay4blhynemcmccg7z7m53pkkqojw6dg3zdzqidytgo62a"
      },
      {
        "action-type": "UPDATED",
        "entity-type": "boot-volumes",
        "entity-uri": "https://iaas.us-phoenix-1.oraclecloud.com/20160918/bootVolumes/ocid1.bootvolume.oc1.phx.abyhqljtjmpf55eh57z73fj7hfgcox3fohw6yaj3qggryamf23dwqkaagr4a",
        "identifier": "ocid1.bootvolume.oc1.phx.abyhqljtjmpf55eh57z73fj7hfgcox3fohw6yaj3qggryamf23dwqkaagr4a"
      },
      {
        "action-type": "UPDATED",
        "entity-type": "instances",
        "entity-uri": "https://iaas.us-phoenix-1.oraclecloud.com/20160918/instances/ocid1.instance.oc1.phx.anyhqljtytsgwaycbkw5karymvcguqxbhx3buztcjjl33vberu62f5ysadca",
        "identifier": "ocid1.instance.oc1.phx.anyhqljtytsgwaycbkw5karymvcguqxbhx3buztcjjl33vberu62f5ysadca"
      }
    ],
    "status": "SUCCEEDED",
    "time-accepted": "2022-09-26T18:05:48.819000+00:00",
    "time-finished": "2022-09-26T18:06:20.904000+00:00",
    "time-started": "2022-09-26T18:06:20.840000+00:00"
  }
}
```


## Types of resources

Getting the resources that can be tagged:

```
windstream-tagging % oci iam tag bulk-edit-tags-resource-type list|grep type
        "resource-type": "DHCPOptions"
        "resource-type": "Policy"
        "resource-type": "AmsSource"
        "resource-type": "DataTransferJob"
        "resource-type": "Compartment"
        "resource-type": "AutonomousDatabase"
        "resource-type": "Image"
        "resource-type": "OdmsConnection"
        "resource-type": "StreamCDNConfig"
        "resource-type": "ServiceGateway"
        "resource-type": "OnsSubscription"
        "resource-type": "BootVolumeBackup"
        "resource-type": "TagNamespace"
        "resource-type": "StreamDistributionChannel"
        "resource-type": "OrmStack"
        "resource-type": "datasciencemodeldeployment"
        "resource-type": "VolumeGroup"
        "resource-type": "InternetGateway"
        "resource-type": "Database"
        "resource-type": "Stream"
        "resource-type": "DbSystem"
        "resource-type": "RemotePeeringConnection"
        "resource-type": "InstancePool"
        "resource-type": "ManagementAgent"
        "resource-type": "CrossConnect"
        "resource-type": "ConsoleHistory"
        "resource-type": "RouteTable"
        "resource-type": "VolumeGroupBackup"
        "resource-type": "Volume"
        "resource-type": "Subnet"
        "resource-type": "Domain"
        "resource-type": "FunctionsApplication"
        "resource-type": "Vcn"
        "resource-type": "datasciencejobrun"
        "resource-type": "MediaWorkflowConfiguration"
        "resource-type": "Group"
        "resource-type": "StreamPackagingConfig"
        "resource-type": "User"
        "resource-type": "OrmPrivateEndpoint"
        "resource-type": "NoSQLTable"
        "resource-type": "Instance"
        "resource-type": "InstanceConfiguration"
        "resource-type": "EventRule"
        "resource-type": "AmsMigration"
        "resource-type": "NatGateway"
        "resource-type": "LocalPeeringGateway"
        "resource-type": "IPSecConnection"
        "resource-type": "VmwareEsxiHost"
        "resource-type": "Vnic"
        "resource-type": "Budget"
        "resource-type": "OrmJob"
        "resource-type": "OdaInstance"
        "resource-type": "PublicIp"
        "resource-type": "VolumeBackup"
        "resource-type": "ApiDeployment"
        "resource-type": "SecurityList"
        "resource-type": "OdmsMigration"
        "resource-type": "BootVolume"
        "resource-type": "AutoScalingConfiguration"
        "resource-type": "CrossConnectGroup"
        "resource-type": "datasciencejob"
        "resource-type": "VmwareSddc"
        "resource-type": "Quota"
        "resource-type": "FunctionsFunction"
        "resource-type": "OdmsAgent"
        "resource-type": "Alarm"
        "resource-type": "Drg"
        "resource-type": "Cpe"
        "resource-type": "MediaWorkflow"
        "resource-type": "Vault"
```

