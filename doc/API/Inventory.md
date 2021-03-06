source: API/Inventory.md

### `get_inventory`

Retrieve the inventory for a device. If you call this without any parameters then you will only get part of the inventory. This is because a lot of devices nest each component, for instance you may initially have the chassis, within this the ports - 1 being an sfp cage, then the sfp itself. The way this API call is designed is to enable a recursive lookup. The first call will retrieve the root entry, included within this response will be entPhysicalIndex, you can then call for entPhysicalContainedIn which will then return the next layer of results.

Route: `/api/v0/inventory/:hostname`

  - hostname can be either the device hostname or the device id

Input:

  - entPhysicalClass: This is used to restrict the class of the inventory, for example you can specify chassis to only return items in the inventory that are labelled as chassis.
  - entPhysicalContainedIn: This is used to retrieve items within the inventory assigned to a previous component, for example specifying the chassis (entPhysicalIndex) will retrieve all items where the chassis is the parent.

Example:
```curl
curl -H 'X-Auth-Token: YOURAPITOKENHERE' https://librenms.org/api/v0/inventory/localhost?entPhysicalContainedIn=65536
```

Output:
```json
{
    "status": "ok",
    "message": "",
    "count": 1,
    "inventory": [
        {
            "entPhysical_id": "2",
            "device_id": "32",
            "entPhysicalIndex": "262145",
            "entPhysicalDescr": "Linux 3.3.5 ehci_hcd RB400 EHCI",
            "entPhysicalClass": "unknown",
            "entPhysicalName": "1:1",
            "entPhysicalHardwareRev": "",
            "entPhysicalFirmwareRev": "",
            "entPhysicalSoftwareRev": "",
            "entPhysicalAlias": "",
            "entPhysicalAssetID": "",
            "entPhysicalIsFRU": "false",
            "entPhysicalModelName": "0x0002",
            "entPhysicalVendorType": "zeroDotZero",
            "entPhysicalSerialNum": "rb400_usb",
            "entPhysicalContainedIn": "65536",
            "entPhysicalParentRelPos": "-1",
            "entPhysicalMfgName": "0x1d6b",
            "ifIndex": "0"
        }
    ]
}
```
