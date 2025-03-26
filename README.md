# Union_Bank_Credit_Card
Credit Card Journey
----PROJECT 1
---Union Bank offers credit cards to its borrowers/customers. The Board of Directors and the SEC have recently 
---approved that Union Bank can begin to venture into offering Loans to its customers as a product.     
/***********************************************TASK**************************************/
---create a database, schemas and tables for this new line of business. 
---The database will be used by the Loan Applications System to store and retrieve data.  

----CREATE DATABASE CALLED UNION BANK
CREATE DATABASE UNION_BANK;
USE UNION_BANK;

----CREATE SCHEMAS AND TABLES 

CREATE SCHEMA BORROWER;
CREATE TABLE BORROWER.BORROWER
(BORROWERID                INT NOT NULL, 
 BORROWERFIRSTNAME         VARCHAR(255) NOT NULL, 
 BORROWERMIDDLENAMEINITIAL CHAR(1) NOT NULL, 
 BORROWERLASTNAME          VARCHAR(255) NOT NULL, 
 DOB                       DATETIME NOT NULL, 
 GENDER                    CHAR(1) NULL, 
 TAXPAYERID_SSN            VARCHAR(9) NOT NULL, 
 PHONENUMBER               VARCHAR(10) NOT NULL, 
 EMAIL                     VARCHAR(255) NOT NULL, 
 CITIZENSHIP               VARCHAR(255) NULL, 
 BENEFICIARYNAME           VARCHAR(255) NULL, 
 ISUSCITIZEN               BIT NULL, 
 CREATEDATE                DATETIME NOT NULL
);


CREATE TABLE BORROWER.BORROWERADDRESS
(ADDRESSID     INT NOT NULL, 
 BORROWERID    INT NOT NULL, 
 STREETADDRESS VARCHAR(255) NOT NULL, 
 ZIP           VARCHAR(5) NOT NULL, 
 CREATEDATE    DATETIME NOT NULL
);


CREATE TABLE CALENDAR
(CALENDARDATE DATETIME NULL);

CREATE TABLE [STATE]
(STATEID    CHAR(2) NOT NULL, 
 STATENAME  VARCHAR(255) NOT NULL, 
 CREATEDATE DATETIME NOT NULL
);


CREATE TABLE US_ZIPCODES
(ISSURROGATEKEY   INT NOT NULL, 
 ZIP              VARCHAR(5) NOT NULL, 
 LATITUDE         FLOAT NULL, 
 LONGITUDE        FLOAT NULL, 
 CITY             VARCHAR(255) NULL, 
 STATE_ID         CHAR(2) NULL, 
 [POPULATION]     INT NULL, 
 DENSITY          DECIMAL(18, 0) NULL, 
 COUNTY_FIPS      VARCHAR(10) NULL, 
 COUNTY_NAME      VARCHAR(255) NULL, 
 COUNTY_NAMES_ALL VARCHAR(255) NULL, 
 COUNTY_FIPS_ALL  VARCHAR(50) NULL, 
 TIMEZONE         VARCHAR(255) NULL, 
 CREATEDATE       DATETIME NOT NULL
);

CREATE SCHEMA LOAN;

CREATE TABLE LOAN.LOANSETUPINFORMATION
(ISSURROGATEKEY           INT NOT NULL, 
 LOANNUMBER               VARCHAR(10) NOT NULL, 
 PURCHASEAMOUNT           NUMERIC(18, 2) NOT NULL, 
 PURCHASEDATE             DATETIME NOT NULL, 
 LOANTERM                 INT NOT NULL, 
 BORROWERID               INT NOT NULL, 
 UNDERWRITERID            INT NOT NULL, 
 PRODUCTID                CHAR(2) NOT NULL, 
 INTERESTRATE             DECIMAL(3, 2) NOT NULL, 
 PAYMENTFREQUENCY         INT NOT NULL, 
 APPRAISALVALUE           NUMERIC(18, 2) NOT NULL, 
 CREATEDATE               DATETIME NOT NULL, 
 LTV                      DECIMAL(4, 2) NOT NULL, 
 FIRSTINTERESTPAYMENTDATE DATETIME NULL, 
 MATURITYDATE             DATETIME NOT NULL
);


CREATE TABLE LOAN.LOANPERIODIC
(ISSUROGATEKEY           INT NOT NULL, 
 LOANNUMBER              VARCHAR(10) NOT NULL, 
 CYCLEDATE               DATETIME NOT NULL, 
 EXTRAMONTHLYPAYMENT     NUMERIC(18, 2) NOT NULL, 
 UNPAIDPRINCIPALBALANCE  NUMERIC(18, 2) NOT NULL, 
 BEGININGSCHEDULEBALANCE NUMERIC(18, 2) NOT NULL, 
 PAIDINSTALLMENT         NUMERIC(18, 2) NOT NULL, 
 INTERESTPORTION         NUMERIC(18, 2) NOT NULL, 
 PRINCIPALPORTION        NUMERIC(18, 2) NOT NULL, 
 ENDSCHEDULEBALANCE      NUMERIC(18, 2) NOT NULL, 
 ACTUALSCHEDULEBALANCE   NUMERIC(18, 2) NOT NULL, 
 TOTALINTERESTACCRUED    NUMERIC(18, 2) NOT NULL, 
 TOTALPRINCIPALSCCRUED   NUMERIC(18, 2) NOT NULL, 
 DEFAULTPENALTY          NUMERIC(18, 2) NOT NULL, 
 DELINQUENCYCODE         INT NOT NULL, 
 CREATEDATE              DATETIME NOT NULL
);


CREATE TABLE LOAN.LU_DELINQUENCY
(DELINQUENCYCODE INT NOT NULL, 
 DELINQUENCY     VARCHAR(255) NOT NULL
);


CREATE TABLE LOAN.LU_PAYMENTFREQUENCY
(PAYMENTFREQUENCY             INT NOT NULL, 
 PAYMENTISMADEEVERY           INT NOT NULL, 
 PAYMENTFREQUENCY_DESCRIPTION VARCHAR(255) NOT NULL
);


CREATE TABLE LOAN.UNDERWRITER
(UNDEWRITERID             INT NOT NULL, 
 UNDERWRITERFIRSTNAME     VARCHAR(255) NULL, 
 UNDERWRITERMIDDLEINITIAL CHAR(1) NULL, 
 UNDERWRITERLASTNAME      VARCHAR(255) NOT NULL, 
 PHONENUMBER              VARCHAR(14) NULL, 
 EMAIL                    VARCHAR(255) NOT NULL, 
 CREATEDATE               DATETIME NOT NULL
);


/*******************BUSINESS RULES TO BE APPLIED TO THE TABLES AND COLUMNS****************************/ 
---BorrowerID Should be the UNIQUE IDENTIFIER OF A RECORD on the borrower table and borrower address table. 
---A borrower Can only have one BorrowerID and a BorrowerID can only be assigned to ONE borrower

ALTER TABLE [BORROWER].[BORROWER]
ADD CONSTRAINT PK_BORROWER_BORROWERID PRIMARY KEY (BORROWERID);
-------------------------------------------------------------------------------------------------------

---A combination of AddressID and BorrowerID Should be the UNIQUE IDENTIFIER OF A RECORD on BorrowerAddress table.

ALTER TABLE [BORROWER].[BORROWERADDRESS]
ADD CONSTRAINT PK_BORROWERADDRESS_BORROWERID_ADDRESSID PRIMARY KEY (BORROWERID, ADDRESSID);

---A combination of LoanNumber and CycleDate should be the UNIQUE IDENTIFIER OF A RECORD on LoanPeriodic table.

ALTER TABLE LOAN.LOANPERIODIC
ADD CONSTRAINT PK_LOANPERIODIC_LOANNUMBER_CYCLEDATE PRIMARY KEY (LOANNUMBER, CYCLEDATE);

---LoanNumber colum Should be the UNIQUE IDENTIFIER OF A RECORD on LoanSetUpInfpormation table.

ALTER TABLE [LOAN].[LOANSETUPINFORMATION]
ADD CONSTRAINT PK_LOANSETUPINFORMATION_LOANNUMBER PRIMARY KEY (LOANNUMBER);

---DelinquencyCode column Should be the UNIQUE IDENTIFIER OF A RECORD on LU_Delinquency table.

ALTER TABLE [LOAN].[LU_DELINQUENCY]
ADD CONSTRAINT PK_LU_DELINQUENCY_DELINQUENCYCODE PRIMARY KEY (DELINQUENCYCODE);

---PaymentFrequency column Should be the UNIQUE IDENTIFIER OF A RECORD on LU_PaymentFrequency table.

ALTER TABLE [LOAN].[LU_PAYMENTFREQUENCY]
ADD CONSTRAINT PK_LU_PAYMENTFREQUENCY_PAYMENTFREQUENCY PRIMARY KEY (PAYMENTFREQUENCY);

---StateID column Should be the UNIQUE IDENTIFIER OF A RECORD on STATE table.

ALTER TABLE [dbo].[STATE]
ADD CONSTRAINT PK_STATE_STATEID PRIMARY KEY (STATEID);

---The underwriterID dhould be the UNIQUE IDENTIFIER OF A RECORD on the Underwriter table. 

ALTER TABLE [LOAN].[UNDERWRITER]
ADD CONSTRAINT PK_UNDERWRITER_UNDERWRITERID PRIMARY KEY ([UNDEWRITERID]);

---The ZIP column of the US_Zipcodes table should be the UNIQUE IDENTIFIER OF A RECORD on this table.

ALTER TABLE [DBO].[US_ZIPCODES]
ADD CONSTRAINT PK_US_ZIPCODES_ZIP PRIMARY KEY (ZIP);

---If no value is inserted on the createdate column in loanperiodic table, then the Create date 
---should default to the current time when the insertion is done

ALTER TABLE [LOAN].[LOANPERIODIC]
ADD CONSTRAINT DF_LOANPERIODIC_CREATEDATE DEFAULT (GETDATE()) FOR [CREATEDATE];


----All borrower should be at least 18 years of as of the date of entry  
ALTER TABLE [BORROWER].[BORROWER]
ADD CONSTRAINT CHK_BORROWER_DOB CHECK([DOB] <= DATEADD(YEAR, -18, GETDATE()));

----The email should containt the '@' symbol in the inserted value     
----alter table your_table 
----add constraint chk_email check (email like '%_@__%.__%')

ALTER TABLE [BORROWER].[BORROWER]
ADD CONSTRAINT CHK_EMAIL CHECK([EMAIL] LIKE '%@%');

---The number of digits entered on the phonenumber column MUST be 10. No less no more     

ALTER TABLE [BORROWER].[BORROWER]
ADD CONSTRAINT CHK_PHONENUMBER CHECK(LEN([PHONENUMBER]) = 10);


---The number of digits entered in the SSN column MUST be 9. No less no more

ALTER TABLE [BORROWER].[BORROWER]
ADD CONSTRAINT CHK_SSN CHECK (LEN([TAXPAYERID_SSN]) = 10);


---If no value is inserted, then the Create date should default to the current 
---time when the insertion is done for borrower and borroweraddress tables

ALTER TABLE [BORROWER].[BORROWER]
ADD CONSTRAINT DF_BORROWER_CREATEDATE DEFAULT (GETDATE()) FOR [CREATEDATE];
---------------------------------------------------------------------------------------
ALTER TABLE [BORROWER].[BORROWERADDRESS]
ADD CONSTRAINT DF_BORROWERADDRESS_CREATEDATE DEFAULT (GETDATE()) FOR [CREATEDATE];

---Borrower ID column in the borrowerAddress table should contain the same values as those
---in the BorrowerID of the Borrower table. In essence, the BorrowerID in both tables should 
---create a relationship in both tables

ALTER TABLE [BORROWER].[BORROWERADDRESS]
ADD CONSTRAINT FK_BORROWERRADDRESS_BORROWERID FOREIGN KEY (BORROWERID) REFERENCES [BORROWER].[BORROWER]([BORROWERID]);

---The ZIP column in the borroweraddress table should contain the same values as those in the ZIP 
---of the US_Zip_Codes table. In essence, the ZIP in both tables should create a relationship in both tables

ALTER TABLE [BORROWER].[BORROWERADDRESS]
ADD CONSTRAINT FK_BORROWERRADDRESS_ZIP FOREIGN KEY (ZIP) REFERENCES [dbo].[US_ZIPCODES]([ZIP]);

---In the loan periodic table, Interestportion+Principalportion =  Paidinstallment

ALTER TABLE [LOAN].[LOANPERIODIC]
ADD CONSTRAINT CHK_PAIDINSTALLMENT_INTERESTPORTION_PRINCIPALPORTION 
CHECK([PAIDINSTALLMENT] = ([PRINCIPALPORTION] + [INTERESTPORTION]));

---Loannumber in Loanperiodic table Is a foreign key referencing the the column LoanNumber in the 
---LoanSetupInformation Table

ALTER TABLE [LOAN].[LOANPERIODIC]
ADD CONSTRAINT FK_LOANPERIODIC_LOANNUMBER FOREIGN KEY (LOANNUMBER) REFERENCES [LOAN].[LOANSETUPINFORMATION](LOANNUMBER);

---Delinquencycode in Loanperiodic table Is a foreign key referencing the the column DelinquencyCode 
---in the  LU_delinquency Table

ALTER TABLE [LOAN].[LOANPERIODIC]
ADD CONSTRAINT FK_LOANPERIODIC_DELINQUENCYCODE FOREIGN KEY (DELINQUENCYCODE) 
REFERENCES [LOAN].[LU_DELINQUENCY]([DELINQUENCYCODE]);

---The Loanterm column in LoanSetUpInformation table should only take the values 35, 30, 15 and 10 

ALTER TABLE [LOAN].[LOANSETUPINFORMATION]
ADD CONSTRAINT CHK_LOANTERM CHECK ([LOANTERM] IN ('35', '30', '15', '10'));

---The interest column in the LoanSetUpInformation should ONLY contain values between 0.01 and 0.30  

ALTER TABLE [LOAN].[LOANSETUPINFORMATION]
ADD CONSTRAINT CHK_INTERESTRATE CHECK ([INTERESTRATE] IN ('>=0.01', '<=0.30'));

---If no value is inserted in createdate of LoanSetUpInformation, then the Create date should default 
---to the current time when the insertion is done 

ALTER TABLE [LOAN].[LOANSETUPINFORMATION]
ADD CONSTRAINT DF_LOANSETUPUNFORMATION_CREATEDATE DEFAULT (GETDATE()) FOR [CREATEDATE];

---BorrowerID column in the LoanSetUpInformation should contain the same values as those in the 
---BorrowerID of the Borrower table. In essence, the BorrowerID in both tables should create a 
---relationship in both tables.

ALTER TABLE [LOAN].[LOANSETUPINFORMATION]
ADD CONSTRAINT FK_LOANSETUPINFORMATION_BORROWERID FOREIGN KEY (BORROWERID) REFERENCES [BORROWER].[BORROWER](BORROWERID);

---PaymentFrequency column in the LoanSetUpInformation Is a foreign key referencing the column
---PaymentFrequency in the  LU_PaymentFrequency table

ALTER TABLE [LOAN].[LOANSETUPINFORMATION]
ADD CONSTRAINT FK_LOANSETUPINFORMATION_PAYMENTFREQUENCY FOREIGN KEY ([PAYMENTFREQUENCY]) 
REFERENCES [LOAN].[LU_PAYMENTFREQUENCY]([PAYMENTFREQUENCY]);

---UnderwriterID column Is a foreign key referencing the column UnderwriterID in the  UnderwriterID Table 

ALTER TABLE [LOAN].[LOANSETUPINFORMATION]
ADD CONSTRAINT FK_LOANSETUPINFORMATION_UNDERWRITERID FOREIGN KEY ([UNDERWRITERID]) 
REFERENCES [LOAN].[UNDERWRITER]([UNDEWRITERID]);

---If no value is inserted in createdate column of the state table, then the Create date should default 
---to the current time when the insertion is done 

ALTER TABLE [dbo].[STATE]
ADD CONSTRAINT DF_STATE_CREATEDATE DEFAULT (GETDATE()) FOR [CREATEDATE];

---The statename column can only take unique values, no duplicates.

ALTER TABLE [dbo].[STATE]
ADD CONSTRAINT UC_STATE_STATENAME UNIQUE ([STATENAME]);

---The Underwriter email should containt the '@' symbol in the inserted value 

ALTER TABLE [LOAN].[UNDERWRITER]
ADD CONSTRAINT CHK_UNDEWRITER_EMAIL CHECK ([EMAIL] LIKE '%@%');

---- alternatively alter table your_table add constraint chk_email check (email like '%_@__%.__%')
--------------------------------------------------------------------------------------------

---If no value is inserted in the createdate column of underwriter table, then the Create date 
---should default to the current time when the insertion is done 

ALTER TABLE [LOAN].[UNDERWRITER]
ADD CONSTRAINT DF_UNDERWRITER_CREATEDATE DEFAULT (GETDATE()) FOR [CREATEDATE];

---If no value is inserted in createdate colum of the US_ZIPCODES table, then the Create date 
---should default to the current time when the insertion is done

ALTER TABLE [dbo].[US_ZIPCODES]
ADD CONSTRAINT DF_US_ZIPCODES_CREATEDATE DEFAULT (GETDATE()) FOR [CREATEDATE];


ALTER TABLE [dbo].[US_ZIPCODES]
ALTER COLUMN [STATE_ID] INT NULL;

---The STATEID column in the US_ZIPCODES table should be a foreign key referencing the column
---UnderwriterID in the  UnderwriterID Table

ALTER TABLE [dbo].[US_ZIPCODES]
ADD CONSTRAINT FK_ZIP_STATEID FOREIGN KEY ([STATE_ID]) REFERENCES [LOAN].[UNDERWRITER]([UNDEWRITERID]);


ALTER TABLE [LOAN].[UNDERWRITER]
DROP CONSTRAINT [PK_UNDERWRITER_UNDERWRITERID]

ALTER TABLE [LOAN].[UNDERWRITER]
ALTER COLUMN [UNDEWRITERID] CHAR (2) NULL; 

ALTER TABLE [LOAN].[UNDERWRITER]
ADD CONSTRAINT PK_UNDERWRITER_UNDERWRITERID PRIMARY KEY UNDERWRITERID


ALTER TABLE [dbo].[US_ZIPCODES]
ADD CONSTRAINT FK_US_ZIPCODES_STATE_ID FOREIGN KEY ([STATE_ID]) REFERENCES [LOAN].[UNDERWRITER]([UNDEWRITERID]);













