# Telecom ERD

### Description of the Use Case
The telecommunications industry is one of the industries where it uses intensive data. It 
stores massive amounts of data from like its customers and subscribers, their calls, SMS, and 
data usage among others. Hence a proper and efficient data management system is needed in 
order to effectively manage these data and deliver better service and customer satisfaction.
In this project, we consider the Mobile Segment Plan of a Telecommunications company. 
We designed a database system containing key data entities under the Mobile Segment Plan
involving customer data, mobile plan, call activities and usage. We implemented this using 
Postgresql and pgAdmin as the tool of choice for development and implementation. We will also 
discuss the ER Model Diagram particularly the relationship among entities, the cardinality, 
connectivity, specialization hierarchies, and other entity relationships.

![alt text](https://github.com/KarlRetumban/Sample1/blob/main/ERD_Telecom.PNG)


### Normalization
All entities in the Mobile Segment Plan ER Model Diagram are in Third Normal Form. There are 
no repeating groups in the table, no partial dependencies, and no transitive dependencies.

#### Create table
~~~~sql
-- subscriber table
CREATE TABLE subscriber (
 subscriber_id VARCHAR(50) NOT NULL,
 firstname VARCHAR(35) NOT NULL,
 lastname VARCHAR(35) NOT NULL,
 gender VARCHAR(35) NOT NULL,
 birthdate DATE NOT NULL,
 email VARCHAR(35) NOT NULL,
 address VARCHAR(100) NOT NULL,
 occupation VARCHAR(100) NOT NULL,
 PRIMARY KEY (subscriber_id)
);
-- plan table
CREATE TABLE plan (
 plan_id VARCHAR(35) NOT NULL,
 plan_name VARCHAR(35) NOT NULL,
 monthly_cost INTEGER NOT NULL,
 call_limit INTEGER NOT NULL,
 sms_limit INTEGER NOT NULL,
 data_limit INTEGER NOT NULL,
 PRIMARY KEY (plan_id)
);
-- contract table
CREATE TABLE contract (
 contract_id VARCHAR(35) NOT NULL,
 subscriber_id VARCHAR(50) NOT NULL,
 phone_num VARCHAR(35) NOT NULL,
 device_id VARCHAR(35) NOT NULL,
 plan_id VARCHAR(35) NOT NULL,
 contractdate_start DATE NOT NULL,
 contractdate_end DATE NOT NULL,
 PRIMARY KEY (contract_id),
 CONSTRAINT FK_contract_subscriber
 FOREIGN KEY (subscriber_id)
 REFERENCES subscriber(subscriber_id),
 CONSTRAINT FK_contract_plan
 FOREIGN KEY (plan_id)
 REFERENCES plan(plan_id)
); 
--phonenumber table
CREATE TABLE phonenumber (
 phone_num VARCHAR(35) NOT NULL,
 simcard_type VARCHAR(35) NOT NULL,
 PRIMARY KEY (phone_num)
);
--calls table
CREATE TABLE calls (
 phone_num VARCHAR(35) NOT NULL,
 call_num2 VARCHAR(35) NOT NULL,
 call_startdatetime TIMESTAMP NOT NULL,
 call_enddatetime TIMESTAMP NOT NULL,
 call_international VARCHAR(35) NOT NULL,
 inbound_outbound VARCHAR(35) NOT NULL,
 PRIMARY KEY (phone_num, call_num2, call_startdatetime, call_enddatetime),
 CONSTRAINT FK_calls_phone_num
 FOREIGN KEY (phone_num)
 REFERENCES phonenumber(phone_num)
);
--sms table
CREATE TABLE sms (
 phone_num VARCHAR(35) NOT NULL,
 sms_num2 VARCHAR(35) NOT NULL,
 sms_datetime TIMESTAMP NOT NULL,
 inbound_outbound VARCHAR(35) NOT NULL,
 sms_size_char INTEGER NOT NULL,
 PRIMARY KEY (phone_num, sms_num2, sms_datetime),
 CONSTRAINT FK_sms_phone_num
 FOREIGN KEY (phone_num)
 REFERENCES phonenumber(phone_num)
);
--datanet table
CREATE TABLE datanet (
 phone_num VARCHAR(35) NOT NULL,
 datetime_start TIMESTAMP NOT NULL,
 datetime_end TIMESTAMP NOT NULL,
 usage_mb INTEGER NOT NULL,
 PRIMARY KEY (phone_num, datetime_start, datetime_end),
 CONSTRAINT FK_datanet_phone_num
 FOREIGN KEY (phone_num)
 REFERENCES phonenumber(phone_num)
);
--device table
CREATE TABLE device (
 contract_id VARCHAR(35) NOT NULL,
 device_id VARCHAR(35) NOT NULL,
 brand VARCHAR(35) NOT NULL,
 storage_capacity INTEGER NOT NULL,
 memory INTEGER NOT NULL,
 device_location VARCHAR(35) NOT NULL,
 device_type VARCHAR(35) NOT NULL,
 model VARCHAR(35) NOT NULL,
 model_year INTEGER NOT NULL,
 PRIMARY KEY (contract_id, device_id)
);
--tablet table
CREATE TABLE tablet (
 device_id VARCHAR(35) NOT NULL,
 screen_size VARCHAR(35) NOT NULL,
 PRIMARY KEY (device_id)
);
--handset table
CREATE TABLE phonenumber (
 phone_num VARCHAR(35) NOT NULL,
 simcard_type VARCHAR(35) NOT NULL,
 PRIMARY KEY (phone_num)
);
--calls table
CREATE TABLE calls (
 phone_num VARCHAR(35) NOT NULL,
 call_num2 VARCHAR(35) NOT NULL,
 call_startdatetime TIMESTAMP NOT NULL,
 call_enddatetime TIMESTAMP NOT NULL,
 call_international VARCHAR(35) NOT NULL,
 inbound_outbound VARCHAR(35) NOT NULL,
 PRIMARY KEY (phone_num, call_num2, call_startdatetime, call_enddatetime),
 CONSTRAINT FK_calls_phone_num
 FOREIGN KEY (phone_num)
 REFERENCES phonenumber(phone_num)
);
--sms table
CREATE TABLE sms (
 phone_num VARCHAR(35) NOT NULL,
 sms_num2 VARCHAR(35) NOT NULL,
 sms_datetime TIMESTAMP NOT NULL,
 inbound_outbound VARCHAR(35) NOT NULL,
 sms_size_char INTEGER NOT NULL,
 PRIMARY KEY (phone_num, sms_num2, sms_datetime),
 CONSTRAINT FK_sms_phone_num
 FOREIGN KEY (phone_num)
 REFERENCES phonenumber(phone_num)
);
--datanet table
CREATE TABLE datanet (
 phone_num VARCHAR(35) NOT NULL,
 datetime_start TIMESTAMP NOT NULL,
 datetime_end TIMESTAMP NOT NULL,
 usage_mb INTEGER NOT NULL,
 PRIMARY KEY (phone_num, datetime_start, datetime_end),
 CONSTRAINT FK_datanet_phone_num
 FOREIGN KEY (phone_num)
 REFERENCES phonenumber(phone_num)
);
--device table
CREATE TABLE device (
 contract_id VARCHAR(35) NOT NULL,
 device_id VARCHAR(35) NOT NULL,
 brand VARCHAR(35) NOT NULL,
 storage_capacity INTEGER NOT NULL,
 memory INTEGER NOT NULL,
 device_location VARCHAR(35) NOT NULL,
 device_type VARCHAR(35) NOT NULL,
 model VARCHAR(35) NOT NULL,
 model_year INTEGER NOT NULL,
 PRIMARY KEY (contract_id, device_id)
);
--tablet table
CREATE TABLE tablet (
 device_id VARCHAR(35) NOT NULL,
 screen_size VARCHAR(35) NOT NULL,
 PRIMARY KEY (device_id)
);
--handset table
CREATE TABLE handset (
 device_id VARCHAR(35) NOT NULL,
 dual_sim VARCHAR(35) NOT NULL,
 color VARCHAR(35) NOT NULL,
 PRIMARY KEY (device_id)
);
--Create index on the Foreign Keys in contract table
CREATE INDEX FK1 ON contract (subscriber_id);
CREATE INDEX FK2 ON contract (phone_num);
CREATE INDEX FK3 ON contract (device_id);
CREATE INDEX FK4 ON contract (plan_id);
--Create index on the Foreign Keys in device table
CREATE INDEX FK5 ON device (contract_id);
CREATE INDEX FK6 ON calls (phone_num);
CREATE INDEX FK7 ON sms (phone_num);
CREATE INDEX FK8 ON datanet (phone_num);
CREATE INDEX FK9 ON tablet (device_id);
CREATE INDEX FK10 ON handset (device_id);
COMMIT;
~~~~

### Data Loading Statements – Importing Data
~~~sql
START TRANSACTION;
COPY SUBSCRIBER (SUBSCRIBER_ID, FIRSTNAME, LASTNAME, GENDER, BIRTHDATE, EMAIL, ADDRESS, OCCUPATION) 
FROM 'C:/Program Files/PostgreSQL/15/data/SUBSCRIBER.csv' ( FORMAT CSV, DELIMITER(','), HEADER );
--should be at least 10 rows. update this.
COPY PLAN 
FROM 'C:/Program Files/PostgreSQL/15/data/PLAN.csv' ( FORMAT CSV, DELIMITER(','), HEADER );
COPY CONTRACT
FROM 'C:/Program Files/PostgreSQL/15/data/CONTRACT.csv' ( FORMAT CSV, DELIMITER(','), HEADER );
COPY PHONENUMBER
FROM 'C:/Program Files/PostgreSQL/15/data/PHONENUMBER.csv' ( FORMAT CSV, DELIMITER(','), HEADER );
COPY DEVICE
FROM 'C:/Program Files/PostgreSQL/15/data/DEVICE.csv' ( FORMAT CSV, DELIMITER(','), HEADER );
COPY HANDSET
FROM 'C:/Program Files/PostgreSQL/15/data/HANDSET.csv' ( FORMAT CSV, DELIMITER(','), HEADER );
COPY TABLET
FROM 'C:/Program Files/PostgreSQL/15/data/TABLET.csv' ( FORMAT CSV, DELIMITER(','), HEADER );
COPY CALLS
FROM 'C:/Program Files/PostgreSQL/15/data/CALLS.csv' ( FORMAT CSV, DELIMITER(','), HEADER );
COPY SMS
FROM 'C:/Program Files/PostgreSQL/15/data/SMS.csv' ( FORMAT CSV, DELIMITER(','), HEADER );
COPY DATANET
FROM 'C:/Program Files/PostgreSQL/15/data/DATANET.csv' ( FORMAT CSV, DELIMITER(','), HEADER );
COMMIT;

~~~~

### Data View
#### Subscriber
![alt text](https://github.com/KarlRetumban/Sample1/blob/main/images/subscriber.PNG)

#### Plan
![alt text](https://github.com/KarlRetumban/Sample1/blob/main/images/plan.PNG)

#### Contract
![alt text](https://github.com/KarlRetumban/Sample1/blob/main/images/contract.PNG)

#### Device 
![alt text](https://github.com/KarlRetumban/Sample1/blob/main/images/device.PNG)

#### Handset
![alt text](https://github.com/KarlRetumban/Sample1/blob/main/images/handset.PNG)

#### Tablet
![alt text](https://github.com/KarlRetumban/Sample1/blob/main/images/tablet.PNG)

#### Phone Number
![alt text](https://github.com/KarlRetumban/Sample1/blob/main/images/phonenumber.PNG)

#### Calls 
![alt text](https://github.com/KarlRetumban/Sample1/blob/main/images/calls.PNG)

#### SMS
![alt text](https://github.com/KarlRetumban/Sample1/blob/main/images/sms.PNG)

#### Data 
![alt text](https://github.com/KarlRetumban/Sample1/blob/main/images/data.PNG)


### Queries
1. This query returns the list of subscribers and the corresponding mobile plan they availed. 
~~~sql
SELECT s.subscriber_id, s.firstname, s.lastname, c.phone_num, n.plan_id, n.plan_name
FROM subscriber AS s
INNER JOIN contract AS c ON s.subscriber_id = c.subscriber_id
INNER JOIN plan AS n ON c.plan_id = n.plan_id;
~~~

2. This query returns the contract details and the type of sim card issued in the contract.
~~~sql
SELECT c.contract_id, c.plan_id, c.contractdate_start, c.contractdate_end, 
 b.phone_num, b.simcard_type
FROM contract as c
LEFT OUTER JOIN phonenumber as b ON c.phone_num = b.phone_num;
~~~

3. This query returns all the subscribers whose data usage exceeds 5000MB. 
~~~sql
SELECT c.plan_id, c.subscriber_id, c.phone_num, SUM(d.usage_mb) AS usage_mb
FROM contract AS C
INNER JOIN calls AS c2 ON c.phone_num = c2.phone_num
LEFT JOIN SMS ON c.phone_num = SMS.phone_num
LEFT JOIN datanet AS d ON c.phone_num = d.phone_num
GROUP BY c.phone_num, c.plan_id, c.subscriber_id
HAVING SUM(usage_mb) > 5000;

~~~
