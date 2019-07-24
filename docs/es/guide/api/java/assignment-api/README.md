# :book: Java API - Device Assignments

<Seo/>

Esta sección contiene la documentación y ejemplos del end point `assignments` de la API de Java de SiteWhere.

Este ejemplo asume que usted obtiene su autenticación del *tenant* ya sea por

```java
ITenantAuthentication tenantAuthentication = SiteWhereClient.defaultTenant();
```

o por la utilización del tenant `default`.

```java
ITenantAuthentication tenantAuthentication = SiteWhereClient.forTenant("token", "auth");
```

## Busqueda de Device Assignments

For searching `DeviceAssignment` you need to provide an instance of `DeviceAssignmentSearchCriteria` to the method
`listDeviceAssignments` of `com.sitewhere.spi.ISiteWhereClient`. The example below shows how you can query SiteWhere
REST API to list the first page of 100 results of assets.

```java
DeviceAssignmentSearchCriteria searchCriteria = new DeviceAssignmentSearchCriteria(1, 100);
DeviceAssignmentResponseFormat format = new DeviceAssignmentResponseFormat();
SearchResults<MarshaledDeviceAssignment> results = client.listDeviceAssignments(tenantAuthentication, searchCriteria, responseFormat);
```

El objeto `DeviceAssignmentSearchCriteria` define los criterios de búsqueda para un `DeviceAssignment`, la siguiente tabla
muestra las propiedades, con su tipo y desdcripción, que pueden ser usadas para filtar los resultados.

| Propiedad              | Type                           | Descripción                                                    |
|:-----------------------|:-------------------------------|:---------------------------------------------------------------|
| setAssignmentStatuses  | `List<DeviceAssignmentStatus>` | Limits search the given list of device assignment statuses.    |
| setDeviceAssignmentTokens          | `List<String>`                 | Limits search the given list of areas tokens.                  |
| setAssetTokens         | `List<String>`                 | Limits search the given list of asset tokens.                  |
| setCustomerTokens      | `List<String>`                 | Limits search the given list of customer tokens.               |
| setDeviceTokens        | `List<String>`                 | Limits search the given list of device tokens.                 |
| setDeviceTypeTokens    | `List<String>`                 | Limits search the given list of device type tokens.            |
| setPageNumber          | `Integer`                      | Indicar el número de pagina del dataset.                       |
| setPageSize            | `Integer`                      | Indicar el número de registros por página.                     |

Además se puede controlar que información es retornada en los resultados proveyendo una instancia de
`DeviceAssignmentResponseFormat`. La siguiente tabla muestra las propiedades que pueden ser establecidas para controlar
el formato del resultado de la respuesta.

| Propiedad                  | Tipo        | Descripción                                                    |
|:---------------------------|:------------|:---------------------------------------------------------------|
| setIncludeDeviceAssignment | `Boolean`   | Indicates if area is to be returned.                           |
| setIncludeAsset            | `Boolean`   | Indicates if asset is to be returned.                          |
| setIncludeCustomer         | `Boolean`   | Indicates if customer is to be returned.                       |
| setIncludeDevice           | `Boolean`   | Indicates if device is to be returned.                         |
| setIncludeDeviceType       | `Boolean`   | Indicates if device type is to be returned.                    |

## Creating an Device Assignment

For creating an `DeviceAssignment` you need to call `createDeviceAssignment` passing the `ITenantAuthentication` object and an
`DeviceAssignmentCreateRequest` build like in the folling example.

```java
String token = "e2ce4ffe-2d9c-4103-b519-1e07c58a2886"; // GUID for the DeviceAssignment
String deviceToken = "15737-UNO-7576364";
String customerToken = "acme";
String areaToken = "southeast";
String assetToken = "derek.adams@sitewhere.com";
DeviceAssignmentCreateRequest.Builder builder =
  new DeviceAssignmentCreateRequest.Builder(deviceToken, customerToken, areaToken, assetToken);
// Build the Create Request
DeviceAssignmentCreateRequest request = builder.build();
request.setToken(token);
// Create the DeviceAssignment
MarshaledDeviceAssignment createdDeviceAssignment = client.createDeviceAssignment(tenantAuthentication, createRequest);
```

## Actualizar un existing Device Assignment

For updating an `DeviceAssignment` you need to call `updateDeviceAssignment` passing the `ITenantAuthentication` object,
the `token` of the existing `DeviceAssignment` and an `DeviceAssignmentCreateRequest` build like in the folling example.

```java
String token = "e2ce4ffe-2d9c-4103-b519-1e07c58a2886"; // GUID for the DeviceAssignment
String deviceToken = "15737-UNO-7576364";
String customerToken = "acme";
String areaToken = "southeast";
String assetToken = "derek.adams@sitewhere.com";
DeviceAssignmentCreateRequest.Builder builder =
  new DeviceAssignmentCreateRequest.Builder(deviceToken, customerToken, areaToken, assetToken);
// Build the Create Request
DeviceAssignmentCreateRequest request = builder.build();
request.setToken(token);
// Build the Create Request
DeviceAssignmentCreateRequest updateRequest = builder.build();
// Update the DeviceAssignment
MarshaledDeviceAssignment updatedDeviceAssignment = client.updateDeviceAssignment(tenantAuthentication, token, updateRequest);
```

## Deleting an existing Device Assignment

For deleting an existing `DeviceAssignment` you need to call `deleteDeviceAssignment` method of `com.sitewhere.spi.ISiteWhereClient`
providing the `token` of the asset you want to delete, like the following example.

```java
String token = "e2ce4ffe-2d9c-4103-b519-1e07c58a2886"; // GUID for the DeviceAssignment
MarshaledDeviceAssignment deletedDeviceAssignment = client.deleteDeviceAssignment(tenantAuthentication, token);
```

## Device Assignment associated API Calls

### Releasing an Device Assignment

The following examples releses an existing associated assignment.

```java
String token = "e2ce4ffe-2d9c-4103-b519-1e07c58a2886"; // GUID for the DeviceAssignment
MarshaledDeviceAssignment releasedAssignment = client.releaseDeviceAssignment(tenantAuthentication, token);
```

### Marking Device Assignment as Missing

The following example marks an existing associated assignment as **missing**.

```java
String token = "e2ce4ffe-2d9c-4103-b519-1e07c58a2886"; // GUID for the DeviceAssignment
MarshaledDeviceAssignment makedMissingAssignment = client.markMissingDeviceAssignment(tenantAuthentication, token);
```

### Quering Alerts associated to a Device Assignment

The following example retrieves firts 100 `DeviceAlert`s associated with an `DeviceAssignment`
from the last year.

```java
String token = "e2ce4ffe-2d9c-4103-b519-1e07c58a2886"; // GUID for the DeviceAssignment
Calendar cal = Calendar.getInstance();

cal.setTime(new Date());
cal.add(Calendar.YEAR, -1);

Date startDate = cal.getTime();
Date endDate = new Date();

DateRangeSearchCriteria searchCriteria = new DateRangeSearchCriteria(1, 100, startDate, endDate);
SearchResults<DeviceAlertWithAsset> alerts = client.listAlertsForDeviceAssignment(tenantAuthentication, token, searchCriteria);
```

### Creating an Alert for a Device Assignment

The following example creates an `Alert` of level `error` for a `DeviceAssignment`, 
with `type` **engine.overheat**, and `message` **Engine Overheat**.

```java
String token = "e2ce4ffe-2d9c-4103-b519-1e07c58a2886"; // GUID for the DeviceAssignment
DeviceAlertCreateRequest.Builder builder = new DeviceAlertCreateRequest.Builder("egine.overheat", "Engine Overheat");

DeviceAlertCreateRequest request = builder
  .error()
  .trackState()
  .build();

DeviceAlertWithAsset alert = client.createAlertForDeviceAssignment(tenantAuthentication, token, request);
```

### Quering Command Invocations associated to a Device Assignment

The following example retrieves firts 100 `DeviceCommandInvocation`s associated with an `DeviceAssignment`
from the last year.

```java
String token = "e2ce4ffe-2d9c-4103-b519-1e07c58a2886"; // GUID for the DeviceAssignment
Calendar cal = Calendar.getInstance();

cal.setTime(new Date());
cal.add(Calendar.YEAR, -1);

Date startDate = cal.getTime();
Date endDate = new Date();

DateRangeSearchCriteria searchCriteria = new DateRangeSearchCriteria(1, 100, startDate, endDate);
SearchResults<DeviceCommandInvocation> commandInvocations = 
  client.listCommandInvocationsForDeviceAssignment(tenantAuthentication, token, searchCriteria);
```

### Creating a Command Invocations for a Device Assignment

The following example creates a `DeviceCommandInvocation` for a `DeviceAssignment`.

```java
String token = "e2ce4ffe-2d9c-4103-b519-1e07c58a2886"; // GUID for the DeviceAssignment
String commandToken = "ping";
String target = "Assignment";
String initiatorId = "REST";

DeviceCommandInvocationCreateRequest.Builder builder =
  new DeviceCommandInvocationCreateRequest.Builder(commandToken, target);
DeviceCommandInvocationCreateRequest request = builder.build();

request.setInitiatorId(initiatorId);
request.setInitiator(CommandInitiator.REST);

DeviceCommandInvocation createdCommandInvocation = client
  .createCommandInvocationForDeviceAssignment(tenantAuthentication, token, request);
```

### Creating an Scheduled Job for a Device Assignment

The followin example creates a `ScheduledJob` for a `DeviceAssignment`.

```java
String token = "e2ce4ffe-2d9c-4103-b519-1e07c58a2886"; // GUID for the DeviceAssignment
String commandToken = "ping";
String target = "Assignment";
String initiatorId = "REST";
String scheduleToken = "de305d54-75b4-431b-adb2-eb6b9e546014";

DeviceCommandInvocationCreateRequest.Builder builder = 
  new DeviceCommandInvocationCreateRequest.Builder(commandToken, target);
DeviceCommandInvocationCreateRequest request = builder.build();

request.setInitiatorId(initiatorId);
request.setInitiator(CommandInitiator.REST);

ScheduledJob createdScheduledJob = client
  .scheduleCommandInvocation(tenantAuthentication, token, scheduleToken, request);
```

### Quering Locations associated to a Device Assignment

The following example retrieves firts 100 `DeviceLocationWithAsset`s associated with an `DeviceAssignment`
from the last year.

```java
String token = "e2ce4ffe-2d9c-4103-b519-1e07c58a2886"; // GUID for the DeviceAssignment
Calendar cal = Calendar.getInstance();

cal.setTime(new Date());
cal.add(Calendar.YEAR, -1);

Date startDate = cal.getTime();
Date endDate = new Date();

DateRangeSearchCriteria searchCriteria = new DateRangeSearchCriteria(1, 100, startDate, endDate);
SearchResults<DeviceLocationWithAsset> locations = client
  .listLocationsForDeviceAssignment(tenantAuthentication, token, searchCriteria);
```

### Creating a Location for a Device Assignment

The following example creates a `DeviceLocation` for a `DeviceAssignment`.

```java
String token = "e2ce4ffe-2d9c-4103-b519-1e07c58a2886"; // GUID for the DeviceAssignment
DeviceLocationCreateRequest.Builder builder = new DeviceLocationCreateRequest.Builder(-27.3313291,-58.961281);
DeviceLocationCreateRequest request = builder.build();

DeviceLocationWithAsset location = client
  .createLocationForDeviceAssignment(tenantAuthentication, token, request);
```

### Quering Measurements associated to a Device Assignment

The following example retrieves firts 100 `DeviceMeasurementWithAsset`s associated with an `DeviceAssignment`
from the last year.

```java
String token = "e2ce4ffe-2d9c-4103-b519-1e07c58a2886"; // GUID for the DeviceAssignment
Calendar cal = Calendar.getInstance();

cal.setTime(new Date());
cal.add(Calendar.YEAR, -1);

Date startDate = cal.getTime();
Date endDate = new Date();

DateRangeSearchCriteria searchCriteria = new DateRangeSearchCriteria(1, 100, startDate, endDate);
SearchResults<DeviceMeasurementWithAsset> measurements = client
  .listMeasurementsForDeviceAssignment(tenantAuthentication, token, searchCriteria);
```

### Creating a Measurement for a Device Assignment

The following example creates a `DeviceMeasurement` for a `DeviceAssignment`, indicating that
**engine.temp** has a value of **50.0**.

```java
DeviceMeasurementCreateRequest.Builder builder = new DeviceMeasurementCreateRequest.Builder();
builder.measurement("engine.temp", 50.0);
DeviceMeasurementCreateRequest request = builder.build();

DeviceMeasurementWithAsset measurement = client
  .createMeasurementForDeviceAssignment(tenantAuthentication, token, request);
```

### Quering Command Responses associated to a Device Assignment

The following example retrieves firts 100 `DeviceCommandResponseWithAsset`s associated with an `DeviceAssignment`
from the last year.

```java
String token = "e2ce4ffe-2d9c-4103-b519-1e07c58a2886"; // GUID for the DeviceAssignment
Calendar cal = Calendar.getInstance();

cal.setTime(new Date());
cal.add(Calendar.YEAR, -1);

Date startDate = cal.getTime();
Date endDate = new Date();

DateRangeSearchCriteria searchCriteria = new DateRangeSearchCriteria(1, 100, startDate, endDate);
SearchResults<DeviceCommandResponseWithAsset> commandResponses = client
  .listCommandResponsesForDeviceAssignment(tenantAuthentication, token, searchCriteria);
```

### Creating a Command Responses for a Device Assignment

```java
String token = "e2ce4ffe-2d9c-4103-b519-1e07c58a2886"; // GUID for the DeviceAssignment
String originatingEventId = "abecfa19-f099-4bc6-b351-a916cd057302"; // GUID of the Originated Event
String responseEventId = "7e841790-057d-4b26-bcbb-a6aaefe36ae8"; // GUID of the Response Event
DeviceCommandResponseCreateRequest request = new DeviceCommandResponseCreateRequest();
request.setResponse("ok");
request.setOriginatingEventId(originatingEventId);
request.setResponseEventId(responseEventId);

DeviceCommandResponseWithAsset commandResponse = client
  .createCommandResponseForDeviceAssignment(tenantAuthentication, token, request);
```

### Quering State Changes associated to a Device Assignment

The following example retrieves firts 100 `DeviceStateChangeWithAsset`s associated with an `DeviceAssignment`
from the last year.

```java
String token = "e2ce4ffe-2d9c-4103-b519-1e07c58a2886"; // GUID for the DeviceAssignment
Calendar cal = Calendar.getInstance();

cal.setTime(new Date());
cal.add(Calendar.YEAR, -1);

Date startDate = cal.getTime();
Date endDate = new Date();

DateRangeSearchCriteria searchCriteria = new DateRangeSearchCriteria(1, 10, startDate, endDate);
SearchResults<DeviceStateChangeWithAsset> stateChanges = client
  .listStateChangesForDeviceAssignment(tenantAuthentication, token, searchCriteria);
```

### Creating State Change for a Device Assignment

The following example creates a `DeviceStateChange` for a `DeviceAssignment`.

```java
DeviceStateChangeCreateRequest request = new DeviceStateChangeCreateRequest();
request.setNewState("PRESENT");
request.setAttribute("Attr");
request.setType("t1");
request.setUpdateState(true);

DeviceStateChangeWithAsset stateChange = client
  .createStateChangeForDeviceAssignment(tenantAuthentication, token, request);
```