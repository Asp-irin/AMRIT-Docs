# Integrating DHIS2 with AMRIT

Integrating **DHIS2** (District Health Information Software 2) with the **AMRIT platform** for reporting involves creating a seamless interface that allows data to flow from AMRIT service lines to DHIS2 while ensuring accuracy, security, and compliance with healthcare standards.&#x20;

### **Integration Architecture**

1. **Data Flow -** Data collected from various AMRIT modules (e.g., Inventory, HWC, Scheduler, MMU) will be exported to DHIS2 for centralized reporting.
2. **Data Exchange Mechanism -** Use Python scripts and **DHIS2 REST APIs** to push data.
3. **Data Mapping -** Map AMRIT’s data structures (e.g., beneficiaries, health programs, inventory) to DHIS2’s **data elements**, **indicators**, and **organization units.**

### **DHIS2** (District Health Information Software 2)&#x20;

**Introduction:**&#x20;

[**DHIS2**](https://dhis2.org/) (District Health Information Software 2) is an open-source software platform designed to manage, analyze, and visualize health data, particularly in low- and middle-income countries. It is widely used in public health systems for data collection, reporting, and analysis, making it an essential tool in monitoring and improving healthcare services.

DHIS2 Download Link: [https://dhis2.org/downloads/](https://dhis2.org/downloads/)

Key Features of DHIS2:

1. Data Collection: Allows easy data entry from multiple sources like mobile devices, tablets, or desktop computers. It supports offline data entry, syncing once the internet is available.
2. Health Monitoring: Tracks a wide range of health indicators such as vaccination coverage, disease outbreaks, maternal health, etc.
3. Data Analysis: Offers powerful tools to generate custom reports, charts, maps, and dashboards. Users can analyze data in real-time to improve decision-making.
4. Customizability: DHIS2 can be tailored to specific health programs or needs by creating custom forms, datasets, and workflows.
5. Integration: Can integrate with other health systems for interoperability and exchange of health data across platforms.

#### **System Requirements**

**Server Requirements:**

* **Operating System**: Linux (Ubuntu recommended), Windows, macOS
* **RAM**: Minimum 8 GB (24 GB recommended for large installations)
* **Processor**: 4-core CPU or higher
* **Disk Space**: 100 GB (depending on data size)

**Software Dependencies:**

* **Java**: Java SE 8 or higher (DHIS2 runs on Java Runtime Environment)
* **Database**: PostgreSQL (Version 10 or higher)
* **Web Server**: Apache Tomcat 8 or higher (for hosting DHIS2)
* **Others**: Nginx (Optional, used for reverse proxy)

#### **Installation Guide:**

**Install Java**

Open the terminal and run the following command to install Java 8:

```
sudo apt update
```

```
sudo apt install openjdk-8-jdk
```

Verify Java installation:

```
java -version
```

**Install PostgreSQL**

Install PostgreSQL database server:

```
sudo apt install postgresql postgresql-contrib
```

Create a new PostgreSQL role and database for DHIS2:

```
sudo -u postgres createuser dhis
```

```
sudo -u postgres createdb -O dhis dhis2
```

Set a password for the PostgreSQL user:

```
sudo -u postgres psql
```

```
ALTER USER dhis WITH PASSWORD 'yourpassword';
```

```
\q
```

Edit `postgresql.conf` to enable remote connections if needed:

```
sudo nano /etc/postgresql/10/main/postgresql.conf
```

Change the following line:

```
listen_addresses = '*'
```

**Install Tomcat**

[**https://tomcat.apache.org/download-90.cgi**](https://tomcat.apache.org/download-90.cgi)

Download and install Tomcat:

```
sudo apt install tomcat9
```

Increase the Tomcat memory allocation by editing the `setenv.sh` file:

```
sudo nano /usr/share/tomcat9/bin/setenv.sh
```

Add the following lines to allocate memory (adjust as needed):

```
export JAVA_OPTS='-Xmx2048m -Xms1024m'
```

**Deploy DHIS2 WAR File**

Download the DHIS2 WAR file from the official DHIS2 site ([https://releases.dhis2.org/](https://releases.dhis2.org/) )

```
wget https://releases.dhis2.org/{version}/dhis2-stable.war -O dhis2.war
```

Move the WAR file to Tomcat's webapps directory:

```
sudo mv dhis2.war /var/lib/tomcat9/webapps/
```

Start Tomcat:

```
sudo systemctl restart tomcat9
```

Access DHIS2 via your browser:

```
http://localhost:8080/dhis
```

#### **Configure DHIS2**

1. After logging in, use the default credentials (`admin` / `district`) to access the platform.
2. Set up your organization structure by navigating to:

```
Maintenance > Organization Units
```

3. Create datasets and assign them to specific organizational units to start data entry and collection.

#### **Using DHIS2**

**Data Entry**

1. To enter data, go to:

```
Data Entry > Select Organization Unit > Select Dataset
```

2. Enter the required data and submit. This data can now be analyzed and visualized in reports.

**Reports & Analysis**

**Reports**:

Navigate to:

```
Reports > Standard Reports
```

Select a report, specify the organization units and period, and generate the report.

**Visualizations**:

Go to:

```
Data Visualizer App > Choose Data
```

Select data elements or indicators to create custom charts, tables, and graphs.

**Maps**

Use the **GIS App** to create maps showing health data geographically:

```
GIS App > Select Layers > Add Data Element
```

#### **User Management**

**Creating Users**

1. Navigate to:

```
Maintenance > Users
```

2. Create users and assign roles based on the permissions required (e.g., data entry, report viewer).

**Setting User Roles**

1. Define user roles in:

```
Maintenance > User Roles
```

2. Assign different levels of access (e.g., system admin, data input user).

#### **Security Best Practices:**

* Always change the default admin password after installation.
* Implement role-based access control (RBAC) to restrict access to sensitive data.
* Regularly back up the PostgreSQL database using:

```
pg_dump -U dhis dhis2 > dhis2-backup.sql
```

* Enable HTTPS for secure communication by configuring SSL in Tomcat or using a reverse proxy like Nginx.

#### **Maintenance & Backup:**

**Updating DHIS2**

1. Download the latest DHIS2 WAR file from the official site.
2. Stop Tomcat:

```
sudo systemctl stop tomcat9
```

3. Replace the old WAR file in `/var/lib/tomcat9/webapps/` with the new one:

```
sudo mv dhis2.war /var/lib/tomcat9/webapps/
```

4. Start Tomcat again:

```
sudo systemctl start tomcat9
```

**Database Backup**

Regular backups are essential. Use the following command to back up the PostgreSQL database:

```
pg_dump -U dhis dhis2 > /path/to/backup/dhis2-backup.sql
```

You can automate this by setting a cron job in Linux:

```
crontab -e
```

Add a backup job to run daily:

```
javascript
```

```
0 2 * * * pg_dump -U dhis dhis2 > /path/to/backup/dhis2-backup-$(date +\%F).sql
```

***

#### **Troubleshooting:**

**Common Issues**

1. **Tomcat Not Starting**:

* Check if the correct Java version is installed.
* Review Tomcat logs in `/var/log/tomcat9/` for specific error messages.

1. **Database Connection Failure**:

* Ensure PostgreSQL is running and accepting connections.
* Verify that database credentials in the DHIS2 configuration file (`dhis.conf`) are correct.

**Logs and Monitoring**

* Monitor Tomcat logs:

```
tail -f /var/log/tomcat9/catalina.out
```

* Check PostgreSQL logs for database-related issues:

```
tail -f /var/log/postgresql/postgresql.log
```

&#x20;

### DHIS2 Architecture

&#x20;<img src="../../.gitbook/assets/Picture 1.png" alt="" data-size="original">&#x20;

DHIS2 Metadata creation: [https://dhis2.org/](https://dhis2.org/)

#### 1. **Introduction to Metadata in DHIS2:**

Metadata in DHIS2 refers to the structural elements that define the data collection process and how data is organized within the system. The key metadata components are:

* **Organization Units**: Geographic or administrative units such as regions, districts, or facilities.
* **Data Elements**: The smallest unit of data, representing what is being collected (e.g., number of vaccinations).
* **Datasets**: A collection of data elements used for data entry.
* **Indicators**: Calculated values that give insight into performance or outcomes (e.g., vaccination coverage).
* **Categories & Category Options**: Define different disaggregation’s (e.g., by age, gender).

#### 2. **Creating Organization Units:**

**Organization Units** represent the hierarchical structure of the health system or organization (e.g., country > region > district > facility). These must be set up before collecting data.

**Steps to Create an Organization Unit:**

1\.     **Navigate to the Maintenance App**:

* Go to `Apps` > `Maintenance`.

2\.     **Create Organization Units**:

* In the left menu, go to `Organization Units` > `Add`.
* Enter the following details:
  * **Name**: Name of the organization unit (e.g., "District Hospital").
  * **Code**: A unique code (optional but recommended).
  * **Parent Unit**: Select the parent unit (e.g., if it's a district, its parent would be a region).
  * **Opening Date**: When this unit became operational.
  * Click **Save**.

3\.     **Organization Unit Hierarchy**:

* You can create more organization units under the parent units to establish a hierarchy (e.g., country > state > district > health center).

#### 3. **Creating Data Elements:**

**Data Elements** are the core items of data collected in DHIS2. For example, "Number of children vaccinated" or "Malaria cases reported."

**Steps to Create a Data Element:**

1\.     **Go to Maintenance**:

* Navigate to `Apps` > `Maintenance`.

2\.     **Add a New Data Element**:

* In the left menu, go to `Data Elements` > `Add`.
* Fill in the following details:
  * **Name**: Name of the data element (e.g., "Children Vaccinated").
  * **Short Name**: A shorter name for quick reference.
  * **Code**: Optional but useful for integration with other systems.
  * **Value Type**: Select the type of value this data will hold (e.g., `Number`, `Text`, `Boolean`).
  * **Aggregation Type**: How the data should be aggregated (e.g., `Sum`, `Average`).
  * **Domain Type**: Choose if it's `Aggregate` (for summary data) or `Tracker` (for individual data).
  * **Category Combination**: Specify how the data is disaggregated (e.g., by age or gender).
  * Click **Save**

#### 3. **Creating Datasets:**

**Datasets** define the collection of data elements that will be used for data entry on a regular basis (e.g., weekly, monthly). Each dataset is linked to specific organization units and time periods.

**Steps to Create a Dataset:**

1\.     **Go to Maintenance**:

* Navigate to `Apps` > `Maintenance`.

2\.     **Add a New Dataset**:

* In the left menu, go to `Datasets` > `Add`.
* Enter the following information:
  * **Name**: The name of the dataset (e.g., "Monthly Vaccination Data").
  * **Period Type**: Select the frequency of data collection (e.g., `Monthly`, `Weekly`, `Daily`).
  * **Category Combination**: Define any disaggregation (e.g., age, sex).
  * **Data Elements**: Add the data elements that will be included in the dataset.
  * **Assigned Organization Units**: Specify which organization units are responsible for data entry (e.g., district health centers).
  * Click **Save**.

#### 5. **Creating Indicators:**

**Indicators** represent calculations based on data elements to provide insights (e.g., "Immunization Coverage"). Indicators can represent percentages, rates, or ratios.

**Steps to Create an Indicator:**

1\.     **Go to Maintenance**:

* Navigate to `Apps` > `Maintenance`.

2\.     **Add a New Indicator**:

* In the left menu, go to `Indicators` > `Add`.
* Enter the following details:
  * **Name**: The name of the indicator (e.g., "Vaccination Coverage").
  * **Short Name**: A short name for easier display.
  * **Numerator**: Define the formula for the numerator, which is usually a data element (e.g., "Number of vaccinated children").
  * **Denominator**: Define the denominator, usually a population data element (e.g., "Total children population").
  * **Indicator Type**: Select the type (e.g., `Percentage`, `Number`).
  * Click **Save**.

#### 6. **Creating Categories and Category Options**

Categories allow you to disaggregate data (e.g., by age, sex). For example, you may want to record "Number of Vaccinations" by both "Age Group" and "Gender."

**Steps to Create Categories & Category Options:**

1\.     **Go to Maintenance**:

* Navigate to `Apps` > `Maintenance`.

2\.     **Create Category Options**:

* In the left menu, go to `Category Options` > `Add`.
* Example: Add options like "Male," "Female" for gender, or "Under 5," "Above 5" for age groups.
* Click **Save**.

3\.     **Create Categories**:

* In the left menu, go to `Categories` > `Add`.
* Define a category (e.g., "Age Group").
* Assign the category options (e.g., "Under 5," "Above 5").
* Click **Save**.

4\.     **Create Category Combinations**:

* In the left menu, go to `Category Combinations` > `Add`.
* Combine multiple categories (e.g., "Age Group" and "Gender").
* Click **Save**.

#### 7. **Assigning Metadata to Organization Units:**

Once the metadata is created, you need to assign it to the relevant organization units to ensure that they can use the data elements, datasets, and indicators during data entry.

**Steps to Assign Metadata:**

1\.     **Go to Maintenance**:

* Navigate to `Apps` > `Maintenance`.

2\.     **Assign Datasets to Organization Units**:

* In the `Datasets` section, open a dataset and select the organization units that will report using this dataset.

3\.     **Assign Data Elements to Categories**:

* In the `Data Elements` section, assign the appropriate category combinations (e.g., Age, Gender) to each data element.

#### 8. **Metadata Export and Import:**

For large implementations, you may want to export metadata from one instance of DHIS2 and import it into another.

**Steps to Export Metadata:**

1\.     **Go to Import/Export App**:

* Navigate to `Apps` > `Import/Export`.

2\.     **Export Metadata**:

* Select `Export Metadata`.
* Choose which metadata (e.g., data elements, datasets) to export.
* Download the file.

**Steps to Import Metadata:**

1\.     **Go to Import/Export App**:

* Navigate to `Apps` > `Import/Export`.

2\.     **Import Metadata**:

* Select `Import Metadata`.
* Upload the metadata file exported earlier.
* Check for conflicts and import.

#### DHIS2 Hosting

Hosting can be done in on-premise servers or in the cloud. DHIS2 is [open source software](http://www.linfo.org/bsdlicense.html) and can be installed at your servers and used for free. You can also go for a professionally managed DHIS2 instance in the cloud. A [managed DHIS2 instance](https://dhis2.org/hosting/) takes care of the backup, security, monitoring and high-speed connectivity aspects of the deployment and allows you to focus on the information system itself.

#### PostgreSQL Examples

**DHIS2: Demographic data:**

Based on the beneficiary registration information first we need to create a data structure (metadata) and then import the required beneficiary demographic data from AMRIT Database to DHIS2 platform using either excel or through Python script we can integrate the data through API.

Query:

`SELECT mb.BeneficiaryID,i_ben_mapping.BenRegId AS mappingBenRegId, i_ben_details.BeneficiaryRegID,`\
`i_ben_details.FirstName,i_ben_details.MiddleName,i_ben_details.LastName,i_ben_details.Gender,`\
`YEAR(i_ben_details.DOB),`\
`IF(i_ben_details.DOB IS NOT NULL,DATE_FORMAT(i_ben_details.DOB, '%Y-01-01'), NULL) AS DOB,`\
`i_ben_mapping.VanID,vanmaster.VanName,f.FacilityName,i_ben_mapping.CreatedDate,CAST(i_ben_mapping.CreatedDate AS DATE),`\
`i_ben_address.PermState,i_ben_address.PermStateId,i_ben_address.PermDistrict,`\
`i_ben_address.PermDistrictId,i_ben_address.PermSubDistrictId`\
`FROM sehatok_db_identity.i_beneficiarymapping i_ben_mapping`\
`INNER join sehatok_db_identity.m_beneficiaryregidmapping mb on mb.BenRegId=i_ben_mapping.BenRegId`\
`INNER JOIN sehatok_db_identity.i_beneficiarydetails i_ben_details ON`\
`i_ben_details.BeneficiaryDetailsId = i_ben_mapping.BenDetailsId`\
`INNER JOIN sehatok_db_identity.i_beneficiaryaddress i_ben_address ON`\
`i_ben_address.BenAddressID = i_ben_mapping.BenAddressId`\
`INNER JOIN sehatok_db_iemr.m_van vanmaster ON vanmaster.VanID = i_ben_mapping.VanID`\
`INNER JOIN sehatok_db_iemr.m_facility f on vanmaster.FacilityID=f.FacilityID`\
`WHERE i_ben_mapping.BenRegId IS NOT NULL`\
`AND i_ben_address.PermStateId IS NOT NULL`\
`AND i_ben_address.PermDistrictId IS NOT NULL`\
`and vanmaster.ProviderServiceMapID = 3;`

Transaction data/ Event data Importing:

Based on the Beneficiary registration we can import the event data in DHIS2 based on specific period-wise:

`SELECT DISTINCT`\
`t.BenCallID,`\
`t.CallID,`\
`t.BeneficiaryRegID,`\
`t.CallTime,`\
`t.CreatedDate,`\
`--  t.CZcallDuration As CallDuration,`\
`TIME_TO_SEC(TIMEDIFF(t.CallEndTime, t.CallTime)) AS CallDurationInSeconds,`\
`CONCAT( UPPER(SUBSTRING(trim(concat(trim(rmu.UserName),"-",t.CallReceivedUserID)), 1, 1)), LOWER(SUBSTRING(trim(concat(trim(rmu.UserName),"-",t.CallReceivedUserID)), 2))) As Received_username,`\
`if(emu.UserName IS NULL , 'Not Assigned-99999',CONCAT( UPPER(SUBSTRING(trim(concat(trim(emu.UserName),"-",t.CallEndUserID)), 1, 1)), LOWER(SUBSTRING(trim(concat(trim(emu.UserName),"-",t.CallEndUserID)), 2))) ) As End_username,`\
t.ReceivedAgentID AS AgentID,\
t.ReceivedRoleName,\
t.IsCalledEarlier,\
t.IsOutbound,\
mct.CallGroupType,\
mct.CallType,\
dt.Call\_Status,\
dt.SessionID,\
dt.Agent\_Disposition\_Category,\
dt.Agent\_Disposition,\
dt.DID\_Number,\
dt.Queue\_time,\
dt.Wrapup\_time,\
dt.Wait\_Time,\
dt.Actual\_Talk\_Time,\
dt.Channel,\
dt.Wrapped\_By\
FROM\
db\_iemr.t\_detailedcallreport dt     left join\
db\_iemr.t\_bencall\_vtbl t  on (t.callid=dt.SessionID and t.ReceivedAgentID=AgentID)\
LEFT JOIN db\_iemr.m\_calltype mct ON mct.CallTypeID = t.CallTypeID\
LEFT JOIN db\_iemr.m\_user\_vtbl rmu on rmu.UserID=t.CallReceivedUserID\
left join db\_iemr.m\_user\_vtbl emu on emu.UserID=t.CallEndUserID\
WHERE t.BeneficiaryRegID IS NOT NULL AND t.IsOutbound IS NOT TRUE AND t.calltypeid IS NOT NULL\
AND (dt.Call\_Start\_Time between '2024-09-01 00:00:00' and '2024-09-17 23:59:59')&#x20;

**Anonymization:**

Anonymization in a database refers to the process of removing or altering personally identifiable identifiable information (PII) so that individuals can’t be identified from the data.

This is crucial for privacy and regulatory compliance, especially dealing with sensitive information sucha as personal details, health information, and financial information.

&#x20;**Masking**

Masking involves replacing sensitive data with a modified version that still looks valid but has no real-world significance. This is often used in systems where data format need to be preserved but the information should not be identificable.



Example:

`SELECT * FROM db_identity.i_beneficiarydetails;`\
`create view i_beneficiarydetails_vtbl as`\
``select `BeneficiaryDetailsId`, `BeneficiaryRegID`, `TitleId`,`Title`,``\
``CONCAT(SUBSTR(FirstName, 1, 1), REPEAT('*', CHAR_LENGTH(FirstName) - 1)) as `FirstName`,``\
``CONCAT(SUBSTR(MiddleName, 1, 1), REPEAT('*', CHAR_LENGTH(MiddleName) - 1)) as `MiddleName`,``\
``CONCAT(SUBSTR(LastName, 1, 1), REPEAT('*', CHAR_LENGTH(LastName) - 1)) as `LastName`,``\
`` `Status`,`GenderId`,`Gender`,`MaritalStatusId`,`MaritalStatus`,`MarriageDate`,`MarriageAge`, ``\
``CONCAT(year(dob),'-00-00 00:00:00') as `DOB`,``\
``CONCAT(SUBSTR(FatherName, 1, 1), REPEAT('*', CHAR_LENGTH(FatherName) - 1)) as `FatherName`,``\
``CONCAT(SUBSTR(MotherName, 1, 1), REPEAT('*', CHAR_LENGTH(MotherName) - 1)) as `MotherName`,``\
``CONCAT(SUBSTR(SpouseName, 1, 1), REPEAT('*', CHAR_LENGTH(SpouseName) - 1)) as `SpouseName`,``\
`` `EmergencyRegistration`,`HealthCareWorkerId`,`HealthCareWorker`,`PlaceOfWork`,`LiteracyStatus`, ``\
`` `educationId`,`education`,`occupationId`,`occupation`,`incomeStatusId`,`incomeStatus`, ``\
`` `communityId`,`community`,`religionId`,`religion`,`preferredLanguageId`,`preferredLanguage`, ``\
`` `sourceOfInfo`,`servicePointId`,`areaId`,`zoneId`,`phcId`,`Remarks`,`familyid`,`HeadofFamily_RelationID`, ``\
`` `HeadofFamily_Relation`,`Others`,`Deleted`,`Processed`,`CreatedBy`,`CreatedDate`,`Reserved`,`ReservedFor`, ``\
`` `ReservedOn`,`ReservedById`,`ModifiedBy`,`LastModDate`,`VanSerialNo`,`VanID`,`VehicalNo`,`ParkingPlaceID`, ``\
`` `SexualOrientationID`,`SexualOrientationType`,`IsHIVPositive`,`HIVStatus`,`SyncedBy`,`SyncedDate`, ``

`` `ReservedForChange`,`MonthlyFamilyIncome`  from `db_identity`.`i_beneficiarydetails`; ``

&#x20;

#### &#x20;Data Integration Process from AMRIT to DHIS2:

**Visualizations:**

After importing the data into DHIS2 application, will run the analytics and create the visualizations based on the requirement.

**Dashboard Sharing:**

After completion of the dashboard will assign the users to the dashboard and provide the required grant access to the user.

A public dashboard URL is also available to share the dashboards publicly anyone has access to the dashboards without any credentials.

Example: [https://dashboards.piramalswasthya.org/dhis-web-public-dashboard/hihlp.html](https://dashboards.piramalswasthya.org/dhis-web-public-dashboard/hihlp.html)

Dashboard PDF format: Dashboard can print using Dashboard layout under more options.&#x20;

Non-PII Data Extraction: DHIS2 reports Data can be extracted in the form of Excel from the Reporting Database and print the report in pdf format.

&#x20;



**MMU/TMC projects:**

Here we have both Print and downloading options available, downloading into excel format and printing in PDF format.

If we want to print the report we can select the print option in the standard reports, it will print into pdf format.&#x20;

**Request Payload Format**

1. API\
   [Reports | DHIS2](https://samiksha.piramalswasthya.org/amrit/dhis-web-reports/index.html#/standard-report/view/pVaaPtcT9SO?\&ou=tLapJv6gIIO)\
   [https://samiksha.piramalswasthya.org/amrit/dhis-web-reports/index.html#/standard-report/view/pVaaPtcT9SO?\&ou=tLapJv6gIIO](https://samiksha.piramalswasthya.org/amrit/dhis-web-reports/index.html#/standard-report/view/pVaaPtcT9SO?\&ou=tLapJv6gIIO)
2. Payload\
   {startDate: "2024-11-18", endDate: "2024-11-19", Program: " Mobile Medical Unit (MMU)", OU:”BSL MMU”,…}
   1. Organisation Unit: BSL MMU
   2. Program: Mobile Medical Unit (MMU)
   3. fileName: "Beneficiary wise report"
   4. Start date: “2024-11-18”
   5. End date: “2024-11-19”

**104 District Report:**

**Request Payload Format**

1. API\
   [Reports | DHIS2](https://samiksha.piramalswasthya.org/amrit/dhis-web-reports/index.html#/standard-report/view/oe6PPF3JELG?\&ou=GDInuuoYIQw)\
   [https://samiksha.piramalswasthya.org/amrit/dhis-web-reports/index.html#/standard-report/view/oe6PPF3JELG?\&ou=GDInuuoYIQw](https://samiksha.piramalswasthya.org/amrit/dhis-web-reports/index.html#/standard-report/view/oe6PPF3JELG?\&ou=GDInuuoYIQw)
2. Payload\
   {startDate: "2024-11-01", endDate: "2024-11-01", Program: "104 service", OU:”Assam”,…}
   1. Organisation Unit: Assam
   2. Program: 104 service
   3. fileName: "104 district report"
   4. Start date: “2024-11-01”
   5. End date: “2024-11-01”

**1097 National Aids helpline:**

**Request Payload Format**

1. API\
   [Reports | DHIS2](https://samiksha.piramalswasthya.org/amrit/dhis-web-reports/index.html#/standard-report/view/pVaaPtcT9SO?\&ou=NQjElqVFZTm)\
   [https://samiksha.piramalswasthya.org/amrit/dhis-web-reports/index.html#/standard-report/view/pVaaPtcT9SO?\&ou=NQjElqVFZTm](https://samiksha.piramalswasthya.org/amrit/dhis-web-reports/index.html#/standard-report/view/pVaaPtcT9SO?\&ou=NQjElqVFZTm)
2. Payload\
   {startDate: "2024-11-18", endDate: "2024-11-19", Program: "1097 National Aids helpline", OU:”BSL MMU”,…}
   1. Organisation Unit: India
   2. Program: 1097 National Aids helpline
   3. fileName: "Beneficiary wise report"
   4. Start date: “2024-11-18”
   5. End date: “2024-11-19”

### References

Document Reference: [https://docs.dhis2.org/en/home.html](https://docs.dhis2.org/en/home.html)

&#x20;

&#x20;

&#x20;
