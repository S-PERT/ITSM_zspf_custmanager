## Application Details
|               |
| ------------- |
|**Generation Date and Time**<br>Wed Sep 16 2026 08:30:12 GMT+0900 (Korean Standard Time)|
|**App Generator**<br>SAP Fiori Application Generator|
|**App Generator Version**<br>1.32.0|
|**Generation Platform**<br>Visual Studio Code|
|**Template Used**<br>List Report Page V4|
|**Service Type**<br>OData URL|
|**Service URL**<br>https://saphana2.s-pert.com:44360/sap/opu/odata4/sap/zsp_itsm_p_custmanager_ui_v4/srvd/sap/zsp_itsm_p_custmanager_ui/0001/|
|**Module Name**<br>zspf_custmanager|
|**Application Title**<br>고객 담당자 마스터|
|**Namespace**<br>zsp.itsm|
|**UI5 Theme**<br>sap_horizon|
|**UI5 Version**<br>1.152.0|
|**Enable TypeScript**<br>False|
|**Add Eslint configuration**<br>True, see https://www.npmjs.com/package/@sap-ux/eslint-plugin-fiori-tools#rules for the eslint rules.|
|**Main Entity**<br>Main|
|**Navigation Entity**<br>None|

## zspf_custmanager

고객 담당자 마스터 관리 애플리케이션

### Starting the generated app

- This app has been generated using the SAP Fiori tools - App Generator, as part of the SAP Fiori tools suite. To launch the generated application, run the following from the generated application root folder:

```
    npm start
```

- It is also possible to run the application using mock data that reflects the OData Service URL supplied during application generation. In order to run the application with Mock Data, run the following from the generated app root folder:

```
    npm run start-mock
```

#### Pre-requisites:

1. Active NodeJS LTS (Long Term Support) version and associated supported NPM version. (See https://nodejs.org)

### Custom Extensions (Selection Fields & Cross-App Navigation)

Fiori Elements 표준 List Report / Object Page V4 템플릿을 기반으로 구현된 고객 담당자 마스터 관리 애플리케이션입니다.

<br>

#### 1) annotation.xml — 솔루션 구분(SolutionFlag) 자식 엔티티 내비게이션 필터 구성

OData V4 환경에서 고객 담당자(`MainType`) 목록을 조회할 때, 담당자가 지원하는 솔루션(`_Solution/SolutionFlag`) 조건으로 검색할 수 있도록 로컬 어노테이션 파일(`annotation.xml`)에 `@UI.SelectionFields`를 정의했습니다.

```xml
<Annotations Target="SAP__self.MainType">
    <Annotation Term="UI.SelectionFields">
        <Collection>
            <PropertyPath>Custno</PropertyPath>
            <PropertyPath>Persno</PropertyPath>
            <PropertyPath>Phone</PropertyPath>
            <PropertyPath>_Solution/SolutionFlag</PropertyPath>
        </Collection>
    </Annotation>
</Annotations>
```

* **효과 및 동작 원리**:
  - `Custno` (고객사코드), `Persno` (담당자코드), `Phone` (전화번호) 기본 필터와 함께, 1:N 관계의 자식 엔티티 경로인 `_Solution/SolutionFlag`를 선언했습니다.
  - Fiori Elements SmartFilterBar에서 솔루션을 선택하여 필터링하더라도 헤더 데이터 중복(Duplicate Rows)이 발생하지 않고 깔끔하게 필터링 조건을 만족하는 담당자만 조회됩니다.

#### 2) manifest.json — Cross-App Navigation 인바운드 등록

타 ITSM 프로그램(거래처 마스터 등)에서 고객담당자 관리로 바로 이동할 수 있도록 Semantic Object Inbound를 구성했습니다.

```json
"crossNavigation": {
  "inbounds": {
    "ZSPF_CUSTMANGER-display": {
      "semanticObject": "ZSPF_CUSTMANGER",
      "action": "display",
      "title": "{{ZSPF_CUSTMANGER-display.flpTitle}}",
      "subTitle": "{{ZSPF_CUSTMANGER-display.flpSubtitle}}",
      "signature": {
        "parameters": {},
        "additionalParameters": "allowed"
      }
    }
  }
}
```

#### 3) 주요 엔티티 및 OData V4 서비스

* **Service Binding**: `ZSP_ITSM_P_CUSTMANAGER_UI_V4` (OData V4)
* **Main Entity**: `Main` (`zsp_itsm_p_custmanager`)
  - `Custno`: 고객사 코드 (Key)
  - `Persno`: 고객 담당자 번호 (Key)
  - `Persname`: 고객 담당자 성명
  - `Phone`: 연락처
  - `Email`: 이메일
  - `_Solution`: 담당 솔루션 목록 (자식 엔티티)
