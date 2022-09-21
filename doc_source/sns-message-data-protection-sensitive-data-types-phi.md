# Sensitive data types: Protected health information \(PHI\)<a name="sns-message-data-protection-sensitive-data-types-phi"></a>

The following table lists and describes the types of protected health information \(PHI\) that Amazon SNS can detect using managed data identifiers\.


| Detection type | Managed data identifier ID | Keyword required | Countries and regions | 
| --- | --- | --- | --- | 
|  Drug Enforcement Agency \(DEA\) Registration Number  |  DrugEnforcementAgencyNumber  |  dea number, dea registration  |  US  | 
| Health Insurance Card Number \(EHIC\) |  HealthInsuranceCardNumber  | assicurazione sanitaria numero, carta assicurazione numero, carte d’assurance maladie, carte européenne d'assurance maladie, ceam, ehic, ehic\#, finlandehicnumber\#, gesundheitskarte, hälsokort, health card, health card number, health insurance card, health insurance number, insurance card number, krankenversicherungskarte, krankenversicherungsnummer, medical account number, numero conto medico, numéro d’assurance maladie, numéro de carte d’assurance, numéro de compte medical, número de cuenta médica, número de seguro de salud, número de tarjeta de seguro, sairaanhoitokortin, sairausvakuutuskortti, sairausvakuutusnumero, sjukförsäkring nummer, sjukförsäkringskort, suomi ehic\-numero, tarjeta de salud, terveyskortti, tessera sanitaria assicurazione numero, versicherungsnummer | EU | 
| Health Insurance Claim Number \(HICN\) |  HealthInsuranceClaimNumber  | health insurance claim number, hic no, hic no\., hic number, hic\#, hicn, hicn\#\., hicno\# | US | 
| Health insurance or medical identification number |  HealthInsuranceNumber  | carte d'assuré social, carte vitale, insurance card | FR | 
| Healthcare Common Procedure Coding System \(HCPCS\) code |  HealthcareProcedureCode  | current procedural terminology, hcpcs, healthcare common procedure coding system | US | 
| Medicare Beneficiary Number \(MBN\) |  MedicareBeneficiaryNumber  | mbi, medicare beneficiary | US | 
| National Drug Code \(NDC\) |  NationalDrugCode  | national drug code, ndc | US | 
| National Provider Identifier \(NPI\) |  NationalProviderId  | hipaa, n\.p\.i, national provider, npi | US | 
| National Health Service \(NHS\) Number |  NhsNumber  | national health service, NHS | GB | 
| Personal Health Number \(PHN\) |  PersonalHealthNumber  | canada healthcare number, msp number, personal healthcare number, phn, soins de santé | CA | 

## Keywords for health insurance and medical identification numbers<a name="sns-managed-data-identifiers-phi-id-keywords"></a>

To detect various types of health insurance and medical identification numbers, Amazon SNS requires a keyword to be in proximity of the numbers\. This includes European Health Insurance Card numbers \(EU, Finland\), health insurance numbers \(France\), Medicare Beneficiary Identifiers \(US\), National Insurance numbers \(UK\), NHS numbers \(UK\), and Personal Health Numbers \(Canada\)\.

The following table lists the keywords that Amazon SNS recognizes for specific countries and regions\.


| Country or region | Keywords | 
| --- | --- | 
| Canada | Canada healthcare number, msp number, personal healthcare number, phn, soins de santé | 
| EU | assicurazione sanitaria numero, carta assicurazione numero, carte d’assurance maladie, carte européenne d'assurance maladie, ceam, ehic, ehic\#, finlandehicnumber\#, gesundheitskarte, hälsokort, health card, health card number, health insurance card, health insurance number, insurance card number, krankenversicherungskarte, krankenversicherungsnummer, medical account number, numero conto medico, numéro d’assurance maladie, numéro de carte d’assurance, numéro de compte medical, número de cuenta médica, número de seguro de salud, número de tarjeta de seguro, sairaanhoitokortin, sairausvakuutuskortti, sairausvakuutusnumero, sjukförsäkring nummer, sjukförsäkringskort, suomi ehic\-numero, tarjeta de salud, terveyskortti, tessera sanitaria assicurazione numero, versicherungsnummer | 
| Finland | ehic, ehic\#, finland health insurance card, finlandehicnumber\#, finska sjukförsäkringskort, hälsokort, health card, health card number, health insurance card, health insurance number, sairaanhoitokortin, sairaanhoitokortin, sairausvakuutuskortti, sairausvakuutusnumero, sjukförsäkring nummer, sjukförsäkringskort, suomen sairausvakuutuskortti, suomi ehic\-numero, terveyskortti | 
| France | carte d'assuré social, carte vitale, insurance card | 
| UK | national health service, NHS | 
| US | mbi, medicare beneficiary | 

### Data identifier ARNs for protected health information data types \(PHI\)<a name="sns-message-data-protection-phi-arns"></a>

The following lists the data identifier Amazon Resource Names \(ARNs\) that can be used in PHI data protection policies\.
+ 

  ```
  arn:aws:dataprotection::aws:data-identifier/DrugEnforcementAgencyNumber-US
  ```
+ 

  ```
  arn:aws:dataprotection::aws:data-identifier/HealthInsuranceCardNumber-EU
  ```
+ 

  ```
  arn:aws:dataprotection::aws:data-identifier/HealthInsuranceClaimNumber-US
  ```
+ 

  ```
  arn:aws:dataprotection::aws:data-identifier/HealthInsuranceNumber-FR
  ```
+ 

  ```
  arn:aws:dataprotection::aws:data-identifier/HealthcareProcedureCode-US
  ```
+ 

  ```
  arn:aws:dataprotection::aws:data-identifier/NationalInsuranceNumber-GB
  ```
+ 

  ```
  arn:aws:dataprotection::aws:data-identifier/NationalProviderId-US
  ```
+ 

  ```
  arn:aws:dataprotection::aws:data-identifier/MedicareBeneficiaryNumber-US
  ```
+ 

  ```
  arn:aws:dataprotection::aws:data-identifier/NationalDrugCode-US
  ```
+ 

  ```
  arn:aws:dataprotection::aws:data-identifier/NhsNumber-GB
  ```
+ 

  ```
  arn:aws:dataprotection::aws:data-identifier/PersonalHealthNumber-CA
  ```