# Main Table
<a name="mainTable"></a>
<table class="table table-striped" style="border-collapse: collapse">
  <tbody>
    <tr>
      <td>*</td>
      <th colspan="9">External Integration Partner</th>
    </tr>
    <tr>
      <th rowspan="8">KT</th>
      <td colspan="2" rowspan="2">&nbsp;</td>
      <th colspan="3">Upstream</th>
      <th colspan="3">Downstream</th>
    </tr>
    <tr>
      <th>Asset</th>
      <th>Inspection</th>
      <th>Service Request</th>
      <th>Asset</th>
      <th>Inspection</th>
      <th>Service Request</th>
    </tr>
    <tr>
      <th rowspan="3">UP</th>
      <th>A</th>
      <th colspan="3" rowspan="3">N/A<br />both upstream</th>
      <td>
        <a href="#create-asset">Create</a><br /><a
          href="#update-asset"
          >Update</a
        >
      </td>
      <td>&nbsp;</td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <th>I</th>
      <td>&nbsp;</td>
      <td>
        <a href="#create-inspection">Create</a><br /><a
          href="#update-inspection"
          >Update</a
        >
      </td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <th>S</th>
      <td>&nbsp;</td>
      <td>&nbsp;</td>
      <td>
        <a href="#create-service-request">Create</a><br /><a
          href="#update-service-request"
          >Update</a
        >
      </td>
    </tr>
    <tr>
      <th rowspan="3">DOWN</th>
      <th>A</th>
      <td>
        <a href="#create-asset-1">Create</a><br /><a
          href="#update-asset-1"
          >Update</a
        >
      </td>
      <td>&nbsp;</td>
      <td>&nbsp;</td>
      <th colspan="3" rowspan="3">N/A<br />both downstream</th>
    </tr>
    <tr>
      <th>I</th>
      <td>&nbsp;</td>
      <td>
        <a href="#create-inspection-1">Create</a><br /><a
          href="#update-inspection-1"
          >Update</a
        >
      </td>
      <td>&nbsp;</td>
    </tr>
    <tr>
      <th>S</th>
      <td>&nbsp;</td>
      <td>&nbsp;</td>
      <td>
        <a href="#create-service-request-1">Create</a><br /><a
          href="#update-service-request-1"
          >Update</a
        >
      </td>
    </tr>
  </tbody>
</table>

## Kaarbontech Upstream
<a name="KUS:createAsset"></a>
### Create Asset
<a href="#mainTable" style="font-size: 0.8em">Back to main table</a>

```mermaid
    sequenceDiagram
    autonumber
    box Kaarbontech<br>upstream
    actor KTU as KT User
    participant KT as Kaarbontech
    end
    box External integration platform<br>downstream
    participant IP as Integration platform
    actor IPU as User
    end
    KTU->>KT:Create asset
    KT->>IP:Creates an Asset
    IP->>KT:Returns new assetID
    Note over IP,KT:KT to reference as externalID
    IP->>IPU:Survey Asset
```

<a name="KUS:updateAsset"></a>
### Update Asset
<a href="#mainTable" style="font-size: 0.8em">Back to main table</a>

```mermaid
    sequenceDiagram
    autonumber
    box Kaarbontech<br>upstream
    actor KTU as KT User
    participant KT as Kaarbontech
    end
    box External integration platform<br>downstream
    participant IP as Integration platform
    actor IPU as User
    end
    KTU->>KT:Update asset
    KT->>IP:Update asset
    Note over KT,IP:Using stored externalID in KT
```

### New Asset with Integration Platform confirmation
<a href="#mainTable" style="font-size: 0.8em">Back to main table</a>

```mermaid
    sequenceDiagram
    autonumber
    box Kaarbontech<br>upstream
    actor KTU as KT User
    participant KT as Kaarbontech
    end
    box External integration platform<br>downstream
    participant IP as Integration platform
    actor IPU as User
    end
    KTU->>KT:Create asset
    KT->>IP:Add Asset for user review
    Note over KT,IP:Add asset to a submission list
    IP->>IPU:Check new assets to add list
    alt
    IPU->>IP:Create new asset
    IP->>KT:Tie up new externalID to KT assetID
    else
    IPU->>IP:Reject new asset
    IP->>KT:Make KT ignore making new asset suggestion
    end
```

<a name="KUS:createInspection"></a>
### Create Inspection
<a href="#mainTable" style="font-size: 0.8em">Back to main table</a>

```mermaid
    sequenceDiagram
    autonumber
    box Kaarbontech<br>upstream
    actor KTU as KT User
    participant KT as Kaarbontech
    end
    box External integration platform<br>downstream
    participant IP as Integration platform
    actor IPU as User
    end
    KTU->>KT:Create inspection
    KT->>IP:Create inspection
    Note over KT,IP:Using stored externalID in KT
    opt Inspection with asset attributes
    KT->>IP:Update asset attributes
    end
```

<a name="KUS:updateInspection"></a>
### Update Inspection
<a href="#mainTable" style="font-size: 0.8em">Back to main table</a>

> [!IMPORTANT]
> **Note** We currently do not store external client inspection id for
  updating. So we would have to rely on external client holding our
  **inspectionID**.


```mermaid
    sequenceDiagram
    autonumber
    box Kaarbontech<br>upstream
    actor KTU as KT User
    participant KT as Kaarbontech
    end
    box External integration platform<br>downstream
    participant IP as Integration platform
    actor IPU as User
    end
    KTU->>KT:Update inspection
    IP->>KT:Fetch external inspectionID
    Note over KT,IP:By looking up our inspectionID
    KT->>IP:Update inspection
    opt Inspection with asset attributes
    IP->>KT:Fetch external assetID for inspection
    KT->>IP:Update asset attributes
    end
```

<a name="KUS:createServiceRequest"></a>
### Create Service Request
<a href="#mainTable" style="font-size: 0.8em">Back to main table</a>

```mermaid
    sequenceDiagram
    autonumber
    box Kaarbontech<br>upstream
    actor KTU as KT User
    participant KT as Kaarbontech
    end
    box External integration platform<br>downstream
    participant IP as Integration platform
    actor IPU as User
    end
    KTU->>KT:Create service request
    KT->>IP:Create service request
    IP->>KT:Return service requestID
    Note over IP,KT:KT to reference as externalID
```

<a name="KUS:updateServiceRequest"></a>
### Update Service Request
<a href="#mainTable" style="font-size: 0.8em">Back to main table</a>

```mermaid
    sequenceDiagram
    autonumber
    box Kaarbontech<br>upstream
    actor KTU as KT User
    participant KT as Kaarbontech
    end
    box External integration platform<br>downstream
    participant IP as Integration platform
    actor IPU as User
    end
    KTU->>KT:Update service request
    KT->>IP:Update service request
    Note over KT,IP:Using stored externalID in KT
    IP->>KT:Return service status
    Note over IP,KT:KT to store external<br>service request status
```
## Kaarbontech Downstream

<a name="KDS:createAsset"></a>
### Create Asset
<a href="#mainTable" style="font-size: 0.8em">Back to main table</a>

```mermaid
    sequenceDiagram
    autonumber
    box External integration platform<br>upstream
    actor IPU as User
    participant IP as Integration platform
    end
    box Kaarbontech<br>downstream
    participant KT as Kaarbontech
    actor KTU as KT User
    end
    IPU->>+IP:Creates an Asset
    IP->>-KT:Create asset
    Note over IP,KT:Storing externalID for<br>reference in KT
```

<a name="KDS:updateAsset"></a>
### Update Asset
<a href="#mainTable" style="font-size: 0.8em">Back to main table</a>

```mermaid
    sequenceDiagram
    autonumber
    box External integration platform<br>upstream
    actor IPU as User
    participant IP as Integration platform
    end
    box Kaarbontech<br>downstream
    participant KT as Kaarbontech
    actor KTU as KT User
    end
    IPU->>+IP:Updates an Asset
    IP->>-KT:Update asset
    Note over IP,KT:Using externalID to<br>reference asset
```

<a name="KDS:createInspection"></a>
### Create Inspection
<a href="#mainTable" style="font-size: 0.8em">Back to main table</a>

```mermaid
    sequenceDiagram
    autonumber
    box External integration platform<br>upstream
    actor IPU as User
    participant IP as Integration platform
    end
    box Kaarbontech<br>downstream
    participant KT as Kaarbontech
    actor KTU as KT User
    end
    IPU->>+IP:Creates an Inspection
    IP->>-KT:Create inspection
    KT->>IP:Return KT inspectionID
    Note over IP,KT:For external integration<br>platform to reference
```

<a name="KDS:updateInspection"></a>
### Update Inspection
<a href="#mainTable" style="font-size: 0.8em">Back to main table</a>

```mermaid
    sequenceDiagram
    autonumber
    box External integration platform<br>upstream
    actor IPU as User
    participant IP as Integration platform
    end
    box Kaarbontech<br>downstream
    participant KT as Kaarbontech
    actor KTU as KT User
    end
    IPU->>+IP:Updates an Inspection
    IP->>-KT:Update inspection
    Note over IP,KT:Using integration platform<br>KT inspectionID to reference
```

### Inspection Downsteam KT, Asset Upstream KT
<a href="#mainTable" style="font-size: 0.8em">Back to main table</a>

```mermaid
    sequenceDiagram
    autonumber
    box External integration platform<br>upstream
    actor IPU as User
    participant IP as Integration platform
    end
    box Kaarbontech<br>downstream
    participant KT as Kaarbontech
    actor KTU as KT User
    end
    KTU->>KT:Creates an Asset
    KT->>IP:Create an Asset
    IP--)KT:Return assetID for<br>KT to reference
    IP->>IPU:Gets asset to survey
    IPU->>IP:Uploads inspection
    par
    IP->>KT:Creates Inspection<br>against asset
    IP->>KT:Update asset attributes
    Note over IP,KT:Where External partner has<br>asset attributes on inspection<br>Eg. Cover Width
    end
```

<a name="KDS:createServiceRequest"></a>
### Create Service Request
<a href="#mainTable" style="font-size: 0.8em">Back to main table</a>

```mermaid
    sequenceDiagram
    autonumber
    box External integration platform<br>upstream
    actor IPU as User
    participant IP as Integration platform
    end
    box Kaarbontech<br>downstream
    participant KT as Kaarbontech
    actor KTU as KT User
    end
    IPU->>+IP:Creates a Service Request
    IP->>-KT:Create service request
    Note over IP,KT:Storing externalID for<br>reference in KT
```

<a name="KDS:updateServiceRequest"></a>
### Update Service Request
<a href="#mainTable" style="font-size: 0.8em">Back to main table</a>

```mermaid
    sequenceDiagram
    autonumber
    box External integration platform<br>upstream
    actor IPU as User
    participant IP as Integration platform
    end
    box Kaarbontech<br>downstream
    participant KT as Kaarbontech
    actor KTU as KT User
    end
    IPU->>+IP:Updates a Service Request
    IP->>-KT:Update service request
    Note over IP,KT:Using externalID to<br>reference service request
    Note right of KT: KT to store external<br>service request status

```
