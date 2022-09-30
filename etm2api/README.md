# ETM to GeoTAX API Field Mapping

Following are the ETM (Enterprise Tax Module) to GeoTAX API field mappings being used in this GeoTAX application:
#### Address Match :

|ETM|GeoTAX API|
|:---------------------------------|:---------------------------------------------------|
|County.Code | taxResponses[n].results[0].jurisdiction.county.code|
|County.Name | taxResponses[n].results[0].jurisdiction.county.name|
|StateCode | taxResponses[n].results[0].jurisdiction.state.code|
|State.Abbreviation | taxResponses[n].results[0].jurisdiction.state.name|
|Confidence | taxResponses[n].results[0].confidence|
|AddressMatch.PBKey | taxResponses[n].results[0].preciselyId|
|AddressMatch.GenRC | taxResponses[n].results[0].matchedAddress.genRC|
|AddressMatch.Lastline | taxResponses[n].results[0].matchedAddress.formattedLocationAddress|
|AddressMatch.LocationCode | taxResponses[n].results[0].matchedAddress.locationCode|
|AddressMatch.MatchCode | taxResponses[n].results[0].matchedAddress.matchCode|
|AddressMatch.NumCandidates | taxResponses[n].results[0].matchedAddress.numCandidates|
|AddressMatch.DataTypeName | taxResponses[n].results[0].matchedAddress.dataTypeName|
|AddressMatch.Firm | taxResponses[n].results[0].matchedAddress.firmName|
|AddressMatch.Urbanization | taxResponses[n].results[0].matchedAddress.urbanization|
|AddressMatch.Zip | taxResponses[n].results[0].matchedAddress.postalCode|
|AddressMatch.Zip4 | taxResponses[n].results[0].matchedAddress.postalCodeExt|
|ProcessedBy | Not Supported (deprecated)|
---

#### Address Match Parsed Elements :

|ETM|GeoTAX API|
|:---------------------------------|:---------------------------------------------------|
|AddressMatch.Street | taxResponses[n].results[0].matchedAddress.street|
|AddressMatch.StreetType | taxResponses[n].results[0].matchedAddress.streetType|
|AddressMatch.HouseNumber | taxResponses[n].results[0].matchedAddress.houseNumber|
|AddressMatch.UnitType | taxResponses[n].results[0].matchedAddress.unitType|
|AddressMatch.UnitNumber | taxResponses[n].results[0].matchedAddress.unit|
|AddressMatch.PreDirectional | taxResponses[n].results[0].matchedAddress.preDirectional|
|AddressMatch.PostDirectional | taxResponses[n].results[0].matchedAddress.postDirectional|
---

#### Input Address :

|ETM|GeoTAX API|
|:---------------------------------|:---------------------------------------------------|
|AddressLine1 | taxResponses[n].results[0].matchedAddress.formattedStreetAddress|
|AddressLine2 | taxResponses[n].results[0].matchedAddress.formattedLocationAddress|
|City | taxResponses[n].results[0].matchedAddress.city|
|StateProvince | taxResponses[n].results[0].matchedAddress.stateName|
|PostalCode | taxResponses[n].results[0].matchedAddress.postalCode|
---

#### Tax Jurisdiction :

|ETM|GeoTAX API|
|:---------------------------------|:---------------------------------------------------|
|GeoTAXKey | taxResponses[n].results[0].jurisdiction.geoTaxKey|
|GeoTAXKey.MatchCode | taxResponses[n].results[0].jurisdiction.geoTaxKeyMatchCode|
|GeoTAXKey.MatchLevel | Not Supported (deprecated)|
|GNISCode | taxResponses[n].results[0].gnisCode|
|Place.ClassCode | taxResponses[n].results[0].jurisdiction.place.classCode|
|Place.Code | taxResponses[n].results[0].jurisdiction.place.code|
|Place.IncorporatedFlag | taxResponses[n].results[0].jurisdiction.place.incorporatedFlag|
|Place.LastAnnexedDate | taxResponses[n].results[0].jurisdiction.place.lastAnnexedDate|
|Place.LastUpdatedDate | taxResponses[n].results[0].jurisdiction.place.lastUpdatedDate|
|Place.LastVerifiedDate | taxResponses[n].results[0].jurisdiction.place.lastVerifiedDate|
|Place.Name | taxResponses[n].results[0].jurisdiction.place.name|
|Place.PointStatus | taxResponses[n].results[0].jurisdiction.place.pointStatus|
|Place.DistanceToBorder | taxResponses[n].results[0].jurisdiction.place.distanceToBorder|
|Place.Confidence | taxResponses[n].results[0].jurisdiction.place.confidence|
---

#### Census :

|ETM|GeoTAX API|
|:---------------------------------|:---------------------------------------------------|
|Census.Block | taxResponses[n].results[0].census.block|
|Census.BlockGroup | taxResponses[n].results[0].census.blockGroup|
|Census.MatchCode | taxResponses[n].results[0].census.matchCode|
|Census.MatchLevel | taxResponses[n].results[0].census.matchLevel|
|Census.Tract | taxResponses[n].results[0].census.tract|
|MCD.Code | taxResponses[n].results[0].census.code|
|MCD.Name | taxResponses[n].results[0].census.name|
|MCD.PointStatus | taxResponses[n].results[0].census.pointStatus|
|MCD.DistanceToBorder | taxResponses[n].results[0].census.distanceToBorder|
|MCD.Confidence | taxResponses[n].results[0].census.confidence|
|CBSA.Code | taxResponses[n].results[0].census.cbsa.code|
|CBSA.Name | taxResponses[n].results[0].census.cbsa.name|
|CBSA.MetroFlag | taxResponses[n].results[0].census.cbsa.metroFlag|
|CBSAD.Code | taxResponses[n].results[0].census.cbsad.code|
|CBSAD.Name | taxResponses[n].results[0].census.cbsad.name|
|CSA.Code | taxResponses[n].results[0].census.csa.code|
|CSA.Name | taxResponses[n].results[0].census.csa.name|
---

#### Latitude/Longitude :

|ETM|GeoTAX API|
|:---------------------------------|:---------------------------------------------------|
|LatLong.MatchCode | taxResponses[n].results[0].latLongFields.matchCode|
|LatLong.MatchLevel | taxResponses[n].results[0].latLongFields.matchLevel|
|Latitude | taxResponses[n].results[0].latLongFields.geometry.coordinates[0]|
|Longitude | taxResponses[n].results[0].latLongFields.geometry.coordinates[1]|
|LatLong | taxResponses[n].results[0].latLongFields.geometry.latLongAltFmtValue|
|LatLong.StreetMatchCode | Not Supported (deprecated)|
|LatLong.StreetMatchLevel | Not Supported (deprecated)|
|Longitude.Directional | Not Supported (deprecated)|
|Latitude.Directional | Not Supported (deprecated)|
---

#### User Defined Boundary :

|ETM|GeoTAX API|
|:---------------------------------|:---------------------------------------------------|
|NumberUserBoundariesFound | taxResponses[n].results[0].numUserBoundariesFound|
|UserBoundaryn.BoundaryDescription | taxResponses[n].results[0].userBoundaries[N].boundaryDescription|
|UserBoundaryn.BoundaryID | taxResponses[n].results[0].userBoundaries[N].boundaryID|
|UserBoundaryn.BufferRelation | taxResponses[n].results[0].userBoundaries[N].bufferRelation|
|UserBoundaryn.DistanceToBorder | taxResponses[n].results[0].userBoundaries[N].distanceToBorder|
|UserBoundaryn.SupplementalBoundaryID | taxResponses[n].results[0].userBoundaries[N].supplementalBoundaryID|
|UserBoundaryn.BoundaryConfidence | taxResponses[n].results[0].userBoundaries[N].boundaryConfidence|
---

#### Insurance Premium Tax Districts :

|ETM|GeoTAX API|
|:---------------------------------|:---------------------------------------------------|
|NumberIPDsFound| taxResponses[n].results[0].numTaxDistrictsFound|
|IPDn.BoundaryBuffer.BufferRelation | taxResponses[n].results[0].ipds[N].boundaryBuffer.bufferRelation.value|
|IPDn.BoundaryBuffer.DistanceToBorder | taxResponses[n].results[0].ipds[N].boundaryBuffer.distanceToBorder.value|
|IPDn.BoundaryConfidence | Not supported (deprecated)|
|IPDn.DistrictID | taxResponses[n].results[0].ipds[N].districtID|
|IPDn.DistrictName | taxResponses[n].results[0].ipds[N].districtName|
|IPDn.DistrictType | taxResponses[n].results[0].ipds[N].districtType.value|
|IPDn.UpdateDate | taxResponses[n].results[0].ipds[N].updateDate|
|IPDn.VersionDate | taxResponses[n].results[0].ipds[N].versionDate|
|IPDn.Notes | taxResponses[n].results[0].ipds[N].notes|
|IPDn.ChangeDate | taxResponses[n].results[0].ipds[N].changeDate|
|IPDn.EffectiveDate | taxResponses[n].results[0].ipds[N].effectiveDate|
|IPDn.ExpirationDate | taxResponses[n].results[0].ipds[N].expirationDate|
|IPDn.FireRate | taxResponses[n].results[0].ipds[N].rates[0].value|
|IPDn.FireFlag | taxResponses[n].results[0].ipds[N].rates[0].format|
|IPDn.CasualtyRate | taxResponses[n].results[0].ipds[N].rates[1].value|
|IPDn.CasualtyFlag | taxResponses[n].results[0].ipds[N].rates[1].format|
|IPDn.VehicleRate | taxResponses[n].results[0].ipds[N].rates[2].value|
|IPDn.VehicleFlag | taxResponses[n].results[0].ipds[N].rates[2].format|
|IPDn.MarineRate | taxResponses[n].results[0].ipds[N].rates[3].value|
|IPDn.MarineFlag | taxResponses[n].results[0].ipds[N].rates[3].format|
|IPDn.HealthRate | taxResponses[n].results[0].ipds[N].rates[4].value|
|IPDn.HealthFlag | taxResponses[n].results[0].ipds[N].rates[4].format|
|IPDn.LifeRate | taxResponses[n].results[0].ipds[N].rates[5].value|
|IPDn.LifeFlag | taxResponses[n].results[0].ipds[N].rates[5].format|
|IPDn.OtherRate | taxResponses[n].results[0].ipds[N].rates[6].value|
|IPDn.OtherFlag | taxResponses[n].results[0].ipds[N].rates[6].format|
|IPDn.MinimumRate | taxResponses[n].results[0].ipds[N].rates[7].value|
|IPDn.MinimumFlag | taxResponses[n].results[0].ipds[N].rates[7].format|
---

#### Payroll Tax Districts :

|ETM|GeoTAX API|
|:---------------------------------|:---------------------------------------------------|
|NumberPAYsFound | taxResponses[n].results[0].numTaxDistrictsFound|
|PAYn.BoundaryBuffer.BufferRelation | taxResponses[n].results[0].ptds[N].boundaryBuffer.bufferRelation.value|
|PAYn.BoundaryBuffer.DistanceToBorder | taxResponses[n].results[0].ptds[N].distanceToBorder.value|
|PAYn.BoundaryConfidence | Not Supported (deprecated)|
|PAYn.DistrictID | taxResponses[n].results[0].ptds[N].districtId|
|PAYn.DistrictName | taxResponses[n].results[0].ptds[N].districtName|
|PAYn.DistrictType | taxResponses[n].results[0].ptds[N].districtType|
|PAYn.ID | taxResponses[n].results[0].ptds[N].id|
|PAYn.MunicipalEMSTax | taxResponses[n].results[0].ptds[N].municipalEMSTax|
|PAYn.MunicipalIncomeTax | taxResponses[n].results[0].ptds[N].municipalIncomeTax|
|PAYn.SchoolDistrictEMSTax | taxResponses[n].results[0].ptds[N].schoolDistrictEMSTax|
|PAYn.SchoolDistrictIncomeTax | taxResponses[n].results[0].ptds[N].schoolDistrictIncomeTax|
---

#### Special Purpose Tax Districts :

|ETM|GeoTAX API|
|:---------------------------------|:---------------------------------------------------|
|NumberSPDsFound | taxResponses[n].results[0].numTaxDistrictsFound|
|SPDn.BoundaryBuffer.BufferRelation | taxResponses[n].results[0].jurisdiction.spds[N].boundaryBuffer.bufferRelation.value|
|SPDn.BoundaryBuffer.DistanceToBorder | taxResponses[n].results[0].jurisdiction.spds[N].distanceToBorder.value|
|SPDn.BoundaryConfidence | Not Supported (deprecated)|
|SPDn.CompiledDate | taxResponses[n].results[0].jurisdiction.spds[N].compiledDate|
|SPDn.DistrictCode | taxResponses[n].results[0].jurisdiction.spds[N].districtCode|
|SPDn.DistrictName | taxResponses[n].results[0].jurisdiction.spds[N].districtName|
|SPDn.DistrictNumber | taxResponses[n].results[0].jurisdiction.spds[N].districtNumber|
|SPDn.EffectiveDate | taxResponses[n].results[0].jurisdiction.spds[N].effectiveDate|
|SPDn.UpdateDate | taxResponses[n].results[0].jurisdiction.spds[N].updateDate|
|SPDn.VersionDate | taxResponses[n].results[0].jurisdiction.spds[N].versionDate|
---

#### Payroll System Tax Code :

|ETM|GeoTAX API|
|:---------------------------------|:---------------------------------------------------|
|NumberPTCsFound | taxResponses[n].results[0].numPTCsFound|
|PTCn.MatchCode | taxResponses[n].results[0].ptc.matchCode|
|PTCn.PayrollCode | taxResponses[n].results[0].ptc.ptcs[N].payrollCode|
|PTCn.PayrollDescription | taxResponses[n].results[0].ptc.ptcs[N].payrollDescription|
|PTCn.PayrollFlag | taxResponses[n].results[0].ptc.ptcs[N].payrollFlag|
---

#### Sales and Use Tax Rates :

|ETM|GeoTAX API|
|:---------------------------------|:---------------------------------------------------|
|TaxRate.RC | taxResponses[n].results[0].salesTax.resultCode|
|Municipal.SalesTaxRate | taxResponses[n].results[0].salesTax.municipalTaxRate|
|County.SalesTaxRate | taxResponses[n].results[0].salesTax.countyTaxRate|
|State.SalesTaxRate | taxResponses[n].results[0].salesTax.stateTaxRate|
|TaxRate.SalesTotal | taxResponses[n].results[0].salesTax.totalTaxRate|
|SPDn.SalesTaxRate | taxResponses[n].results[0].salesTax.spdsTax[N].taxRate|
|Municipal.UseTaxRate| taxResponses[n].results[0].useTax.municipalTaxRate|
|County.UseTaxRate | taxResponses[n].results[0].useTax.countyTaxRate|
|State.UseTaxRate | taxResponses[n].results[0].useTax.stateTaxRate|
|TaxRate.UseTotal | taxResponses[n].results[0].useTax.totalTaxRate|
|SPDn.UseTaxRate | taxResponses[n].results[0].useTax.spdsTax[N].taxRate|
---

#### Auxiliary File :

|ETM|GeoTAX API|
|:---------------------------------|:---------------------------------------------------|
|AuxiliaryData.AuxiliaryFile | Not Supported (deprecated)|
|AuxiliaryData.StateFile | Not Supported (deprecated)|
---
