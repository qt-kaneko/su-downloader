## API
### Search autocomplete
```xml
POST https://orcaservice.samsungmobile.com:10443/downloadcenter.asmx
Content-Type: text/xml
SOAPAction: http://orcaservice.samsungmobile.com/GetModelListBykeyword

<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <GetModelListBykeyword xmlns="http://orcaservice.samsungmobile.com/">
      <keyword>{keyword}</keyword>
      <region></region>
    </GetModelListBykeyword>
  </soap:Body>
</soap:Envelope>
```
| Variable    | Description          |
| ----------- | -------------------- |
| `{keyword}` | User's search query. |
<details>
  <summary>Response</summary>

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <soap:Envelope
    xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <soap:Body>
      <GetModelListBykeywordResponse
        xmlns="http://orcaservice.samsungmobile.com/">
        <GetModelListBykeywordResult>
          <ResultInfo>
            <ResultCode>0</ResultCode>
            <ResultDesc>Success</ResultDesc>
          </ResultInfo>
          <Region />
          <Keyword>XE500T</Keyword>
          <DerivedModel>XE500T1C-A01AE</DerivedModel> <!-- Autocomplete suggestion -->
          <!-- ... -->
        </GetModelListBykeywordResult>
      </GetModelListBykeywordResponse>
    </soap:Body>
  </soap:Envelope>
  ```
</details>

### Available OS
```xml
POST https://orcaservice.samsungmobile.com:10443/downloadcenter.asmx
Content-Type: text/xml
SOAPAction: http://orcaservice.samsungmobile.com/GetSupportedOSList

<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <GetSupportedOSList xmlns="http://orcaservice.samsungmobile.com/">
      <modelName>{modelName}</modelName>
    </GetSupportedOSList>
  </soap:Body>
</soap:Envelope>
```
| Variable      | Description                                             |
| ------------- | ------------------------------------------------------- |
| `{modelName}` | [Model name](#search-autocomplete) from previous steps. |
<details>
  <summary>Response</summary>

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <soap:Envelope
    xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <soap:Body>
      <GetSupportedOSListResponse
        xmlns="http://orcaservice.samsungmobile.com/">
        <GetSupportedOSListResult>
          <ResultInfo>
            <ResultCode>0</ResultCode>
            <ResultDesc>Success</ResultDesc>
          </ResultInfo>
          <OS>W8</OS> <!-- Available OS -->
          <!-- ... -->
        </GetSupportedOSListResult>
      </GetSupportedOSListResponse>
    </soap:Body>
  </soap:Envelope>
  ```
</details>

### Drivers list
#### Get model code
```xml
POST https://orcaservice.samsungmobile.com:10443/downloadcenter.asmx
Content-Type: text/xml
SOAPAction: http://orcaservice.samsungmobile.com/GetModelCode

<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <GetModelCode xmlns="http://orcaservice.samsungmobile.com/">
      <derivedModel>{derivedModel}</derivedModel>
    </GetModelCode>
  </soap:Body>
</soap:Envelope>
```
| Variable         | Description                                             |
| ---------------- | ------------------------------------------------------- |
| `{derivedModel}` | [Model name](#search-autocomplete) from previous steps. |
<details>
  <summary>Response</summary>

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <soap:Envelope
    xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <soap:Body>
      <GetModelCodeResponse
        xmlns="http://orcaservice.samsungmobile.com/">
        <GetModelCodeResult>
          <ResultInfo>
            <ResultCode>0</ResultCode>
            <ResultDesc>Success</ResultDesc>
          </ResultInfo>
          <ModelCode>HXJ291</ModelCode> <!-- Model code -->
        </GetModelCodeResult>
      </GetModelCodeResponse>
    </soap:Body>
  </soap:Envelope>
  ```
</details>

#### Get drivers lists
```http
POST https://orcaservice.samsungmobile.com:10443/api/ORCAHDMInfo
Content-Type: application/json

{
  "XMLVERSION": "3.0",
  "SN": "{sn}",
  "REGION": "",
  "OS": "{os}",
  "LANG": "",
  "CLIENTVERSION": "{clientversion}",
  "ETC": "",
  "TESTMODE": "NONE"
}
```
| Variable          | Description                                        |
| ----------------- | -------------------------------------------------- |
| `{sn}`            | [Model code](#get-model-code) from previous steps. |
| `{os}`            | [OS](#available-os) from previous steps.           |
| `{clientversion}` | Client version (i.e. 3.0.108.0).                   |
<details>
  <summary>Response</summary>

  ```jsonc
  {
    "XmlVersion": "3.0",
    "Result": "SUCC",
    "SN": "HXJ291",
    "REGION": "",
    "OS": "W8",
    "LANG": "",
    "CLIENTVERSION": "3.0.108.0",
    "HDM": {
      "REV": "930914f031f6c367652185fff1195fd5988cab77",
      "ITEM": [
        {
          "ID": "SGL6868A04-C01-R005-S0001",
          "HASHCODE": "33EAAA90",
          "FURL": "http://appmanager.samsungmobile.com/dl/bom/SGL6868A04-C01-R005-S0001.XML" /* Drivers list */
        },
        /* ... */
      ]
    },
    "ADDSW": null,
    "PATCH": null,
    "DOWNLOADONLY": null,
    "SELFUPDATE": null,
    "POLICY": null,
    "POLICYURL": null,
    "CLIENTPOLICY": null,
    "FailInfo": null
  }
  ```
</details>

#### Individual driver
[Drivers list](#get-drivers-lists) from previous steps.
```xml
<MaxList>
  <Head>
    <BOMID>SGL6868A04-C01-R005-S0001</BOMID>
    <CISCode/>
    <Product/>
    <Project>William-11</Project>
    <Model>WILLIAM</Model>
    <DevStep>MP200</DevStep>
    <BaseMRT>MRT6868A01</BaseMRT>
    <BaseBOM/>
    <Region>AU/NZ/EE/PL/BE/NL</Region>
    <OS>W8ST32</OS>
    <Language>POL/FRE/DUT/GER/ENG</Language>
    <ROLString>ALL</ROLString>
    <Date>2013-03-21 오전 9:04:12</Date>
    <Time>2013-03-21 오전 9:04:12</Time>
    <Test>Yes</Test>
  </Head>
  <Item>
    <CISCode>BASW-83835A06</CISCode>
    <ItemType>SOFTWARE</ItemType>
    <DisplayName>Wacom Digitizer driver for William(I2C) 7.1.0.5 WHQL</DisplayName> <!-- Name -->
    <Region>DNC</Region>
    <OS>DONCR</OS>
    <Lang>DNC</Lang>
    <ROLString>ALL</ROLString>
    <InstallType>PSTEXE</InstallType>
    <InstallPath>BASW-83835A\BASW-83835A05.ZIP</InstallPath>
    <InstallFile>Setup.exe</InstallFile>
    <InstallPara1>/s</InstallPara1>
    <InstallPara2>/pbr</InstallPara2>
    <InstallOrgFileSize>50401201</InstallOrgFileSize>
    <InstallFileSize>17712640</InstallFileSize>
    <ImageCate>C2P1</ImageCate>
    <ImageType>GCP</ImageType>
    <ImageSequence>21540</ImageSequence>
    <MediaType>SM1</MediaType>
    <MediaSubCate>ITMRQR</MediaSubCate>
    <MediaSequence>320</MediaSequence>
    <CheckType>FileVer</CheckType>
    <CheckRoot>%ProgramFiles%\Tablet\ISD\ISD_Tablet.exe</CheckRoot>
    <VerifyAttribute>7.1.0.5</VerifyAttribute>
    <VerifyPara1/>
    <VerifyPara2/>
    <System/>
    <Selectable>N</Selectable>
    <AND/>
    <XOR/>
    <FURL>http://orcaservice.samsungmobile.com/FileDownloader.aspx?FILENAME=BASW-83835A05.ZIP</FURL> <!-- Download URL -->
    <MultiLangDisplayName>
      <Default>ENG</Default>
      <Value>
        <Lang>ENG</Lang>
        <Str>Digitizer Driver</Str> <!-- Localized name -->
      </Value>
      <!-- ... -->
    </MultiLangDisplayName>
    <Version>7.1.0.5</Version>
    <DDesc>
      <Default>ENG</Default>
      <Value>
        <Lang>ENG</Lang>
        <Str>Installs the digitizer driver and utility.</Str> <!-- Localized description -->
      </Value>
    </DDesc>
    <SWCate2>Driver</SWCate2>
    <Keyword1>Device&Driver</Keyword1>
    <Keyword2>Digitizer</Keyword2>
    <Keyword3>Wacom</Keyword3>
    <AdURL>
      <URL/>
      <FromDate>1900-01-01 오전 12:00:00</FromDate>
      <ToDate>1900-01-01 오전 12:00:00</ToDate>
    </AdURL>
    <Thumbnail/>
    <Screenshot1/>
    <Screenshot2/>
    <Screenshot3/>
  </Item>
  <!-- ... -->
</MaxList>
```