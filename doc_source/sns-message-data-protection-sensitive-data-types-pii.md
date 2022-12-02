# Sensitive data types: Personally identifiable information \(PII\)<a name="sns-message-data-protection-sensitive-data-types-pii"></a>

The following table lists and describes the types of personally identifiable information \(PII\) that Amazon SNS can detect using managed data identifiers\.


| Detection type | Managed data identifier ID | Keyword required | Additional information | Countries and regions | 
| --- | --- | --- | --- | --- | 
| Birth date | DateOfBirth | dob, date of birth, birthdate, birth date, birthday, b\-day, bday | Support includes most date formats, such as all digits and combinations of digits and names of months\. Date components can be separated by spaces, slashes \(/\), or hyphens \(‐\)\. | Any | 
| Código de Endereçamento Postal \(CEP\) |  CepCode  | cep, código de endereçamento postal, codigo de endereçamento postal | – | Brazil | 
| Cadastro Nacional da Pessoa Jurídica \(CNPJ\) |  Cnpj  | cadastro nacional da pessoa jurídica, cadastro nacional da pessoa juridica, cnpj | – | Brazil | 
| Cadastro de Pessoas Físicas \(CPF\) |  CpfCode  | Cadastro de pessoas fisicas, cadastro de pessoas físicas, cadastro de pessoa física, cadastro de pessoa fisica, cpf | – | Brazil | 
| Driver’s license identification number |  DriversLicense  | Yes, see [Keywords for driver’s license identification numbers](#sns-managed-data-identifiers-pii-dl-keywords)\. | – | Australia, Austria, Belgium, Bulgaria, Canada, Croatia, Cyprus, Czech Republic, Denmark, Estonia, Finland, France, Germany, Greece, Hungary, Ireland, Italy, Latvia, Lithuania, Luxembourg, Malta, Netherlands, Poland, Portugal, Romania, Slovakia, Slovenia, Spain, Sweden, UK, US | 
| Electoral roll number |  ElectoralRollNumber  | electoral\#, electoral \#, electoralnumber, electoral number, electoralroll\#, electoral roll\#, electoral roll \#, electoral roll no\., electoral roll number, electoralrollno | – | UK | 
| Individual taxpayer identification |  IndividualTaxIdentificationNumber  | Yes, see [Keywords for taxpayer identification and reference numbers](#sns-managed-data-identifiers-financial-tin-keywords)\. | – | US | 
| National Institute for Statistics and Economic Studies \(INSEE\) |  InseeCode  | Yes, see [Keywords for national identification numbers](#sns-managed-data-identifiers-pii-natlid-keywords)\. | – | France | 
| National identification number |  NationalIdentificationNumber  | Yes, see [Keywords for national identification numbers](#sns-managed-data-identifiers-pii-natlid-keywords)\. | This includes Documento Nacional de Identidad \(DNI\) identifiers \(Spain\), Codice fiscale codes \(Italy\), and National Identity Card numbers \(German\)\. | Germany, Italy, Spain | 
| National Insurance Number \(NINO\) |  NationalInsuranceNumber  | insurance no\., insurance number, insurance\#, national insurance number, nationalinsurance\#, nationalinsurancenumber, nin, nino | – | UK | 
| Número de identidad de extranjero \(NIE\) |  NieNumber  | Yes, see [Keywords for taxpayer identification and reference numbers](#sns-managed-data-identifiers-financial-tin-keywords)\. | – | Spain | 
| Número de Identificación Fiscal \(NIF\) |  NifNumber  | Yes, see [Keywords for taxpayer identification and reference numbers](#sns-managed-data-identifiers-financial-tin-keywords)\. | – | Spain | 
| Passport number |  PassportNumber  | Yes, see [Keywords for passport numbers](#sns-managed-data-identifiers-pii-passport-keywords)\. | – | Canada, France, Germany, Italy, Spain, UK, US | 
| Permanent residence number |  PermanentResidenceNumber  | carte résident permanent, numéro carte résident permanent, numéro résident permanent, permanent resident card, permanent resident card number, permanent resident no, permanent resident no\., permanent resident number, pr no, pr no\., pr non, pr number, résident permanent no\., résident permanent non | – | Canada | 
| Phone number |  PhoneNumber  |  Brazil: keywords also include: cel, celular, fone, móvel, número residencial, numero residencial, telefone Others: cell, contact, fax, fax number, mobile, phone, phone number, tel, telephone, telephone number  | This includes toll\-free numbers in the US and fax numbers\. If a keyword is in proximity of the data, the number doesn’t have to include a country code\. If a keyword isn’t in proximity of the data, the number has to include a country code\. | Brazil, Canada, France, Germany, Italy, Spain, UK, US | 
| Postal Code |  PostalCode  | No | – | Canada | 
| Registro Geral \(RG\) |  RgNumber  | Yes, see [Keywords for national identification numbers](#sns-managed-data-identifiers-pii-natlid-keywords)\. | – | Brazil | 
| Social Insurance Number \(SIN\) |  SocialInsuranceNumber  | canadian id, numéro d'assurance sociale, social insurance number, sin | – | Canada | 
| Social Security number \(SSN\) |  Ssn  | Spain – número de la seguridad social, social security no\., social security no\. número de la seguridad social, social security number, socialsecurityno\#, ssn, ssn\# US – social security, ss\#, ssn  | – | Spain, US | 
| Taxpayer identification or reference number |  TaxId  | Yes, see [Keywords for taxpayer identification and reference numbers](#sns-managed-data-identifiers-financial-tin-keywords)\. | This includes TIN \(France\); Steueridentifikationsnummer \(Germany\); CIF \(Spain\); and TRN, UTR \(UK\)\. | France, Germany, Spain, UK | 
| US postal code |  ZipCode  | zip code, zip\+4 | – | US | 
| Mailing address |  Address  | No | Although a keyword isn't required, detection requires the address to include the name of a city or place and a ZIP or Postal Code\. | Australia, Canada, France, Germany, Italy, Spain, UK, US | 
| Electronic mail address |  EmailAddress  | No | – | Any | 
| Global Positioning System \(GPS\) coordinates |  LatLong  | coordinate, coordinates, lat long, latitude longitude, location, position | Amazon SNS can detect GPS coordinates if the latitude and longitude coordinates are stored as a pair and they're in Decimal Degrees \(DD\) format, for example, 41\.948614,\-87\.655311\. Support doesn't include coordinates in Degrees Decimal Minutes \(DDM\) format, for example 41°56\.9168'N 87°39\.3187'W, or Degrees, Minutes, Seconds \(DMS\) format, for example 41°56'55\.0104"N 87°39'19\.1196"W\. | Any | 
| Full name |  Name  | No | Amazon SNS can detect full names only\. Support is limited to Latin character sets\. | Any | 
| Vehicle identification number \(VIN\) |  VehicleIdentificationNumber  |  Fahrgestellnummer, niv, numarul de identificare, numarul seriei de sasiu, serie sasiu, numer VIN, Número de Identificação do Veículo, Número de Identificación de Automóviles, numéro d'identification du véhicule, vehicle identification number, vin, VIN numeris | Amazon SNS can detect VINs that consist of a 17\-character sequence and adhere to the ISO 3779 and 3780 standards\. These standards were designed for worldwide use\. | Any | 

## Keywords for driver’s license identification numbers<a name="sns-managed-data-identifiers-pii-dl-keywords"></a>

To detect various types of driver’s license identification numbers, Amazon SNS requires a keyword to be in proximity of the numbers\. The following table lists the keywords that Amazon SNS recognizes for specific countries and regions\.


| Country or region | Keywords | 
| --- | --- | 
| Australia | dl\# dl:, dl :, dlno\# driver licence, driver license, driver permit, drivers lic\., drivers licence, driver's licence, drivers license, driver's license, drivers permit, driver's permit, drivers permit number, driving licence, driving license, driving permit | 
| Austria | führerschein, fuhrerschein, führerschein republik österreich, fuhrerschein republik osterreich | 
| Belgium | fuehrerschein, fuehrerschein\- nr, fuehrerscheinnummer, fuhrerschein, führerschein, fuhrerschein\- nr, führerschein\- nr, fuhrerscheinnummer, führerscheinnummer, numéro permis conduire, permis de conduire, rijbewijs, rijbewijsnummer | 
| Bulgaria | превозно средство, свидетелство за управление на моторно, свидетелство за управление на мпс, сумпс, шофьорска книжка | 
| Canada | dl\#, dl:, dlno\#, driver licence, driver licences, driver license, driver licenses, driver permit, drivers lic\., drivers licence, driver's licence, drivers licences, driver's licences, drivers license, driver's license, drivers licenses, driver's licenses, drivers permit, driver's permit, drivers permit number, driving licence, driving license, driving permit, permis de conduire | 
| Croatia | vozačka dozvola | 
| Cyprus | άδεια οδήγησης | 
| Czech Republic | číslo licence, císlo licence řidiče, číslo řidičského průkazu, ovladače lic\., povolení k jízdě, povolení řidiče, řidiči povolení, řidičský prúkaz, řidičský průkaz | 
| Denmark | kørekort, kørekortnummer | 
| Estonia | juhi litsentsi number, juhiloa number, juhiluba, juhiluba number | 
| Finland | ajokortin numero, ajokortti, förare lic\., körkort, körkort nummer, kuljettaja lic\., permis de conduire | 
| France | permis de conduire | 
| Germany | fuehrerschein, fuehrerschein\- nr, fuehrerscheinnummer, fuhrerschein, führerschein, fuhrerschein\- nr, führerschein\- nr, fuhrerscheinnummer, führerscheinnummer | 
| Greece | δεια οδήγησης, adeia odigisis | 
| Hungary | illesztőprogramok lic, jogosítvány, jogsi, licencszám, vezető engedély, vezetői engedély | 
| Ireland | ceadúnas tiomána | 
| Italy | patente di guida, patente di guida numero, patente guida, patente guida numero | 
| Latvia | autovadītāja apliecība, licences numurs, vadītāja apliecība, vadītāja apliecības numurs, vadītāja atļauja, vadītāja licences numurs, vadītāji lic\. | 
| Lithuania | vairuotojo pažymėjimas | 
| Luxembourg | fahrerlaubnis, führerschäin | 
| Malta | liċenzja tas\-sewqan | 
| Netherlands | permis de conduire, rijbewijs, rijbewijsnummer | 
| Poland | numer licencyjny, prawo jazdy, zezwolenie na prowadzenie | 
| Portugal | carta de condução, carteira de habilitação, carteira de motorist, carteira habilitação, carteira motorist, licença condução, licença de condução, número de licença, número licença, permissão condução, permissão de condução | 
| Romania | numărul permisului de conducere, permis de conducere | 
| Slovakia | číslo licencie, číslo vodičského preukazu, ovládače lic\., povolenia vodičov, povolenie jazdu, povolenie na jazdu, povolenie vodiča, vodičský preukaz | 
| Slovenia | vozniško dovoljenje | 
| Spain | carnet conducer, el carnet de conducer, licencia conducer, licencia de manejo, número carnet conducer, número de carnet de conducer, número de permiso conducer, número de permiso de conducer, número licencia conducer, número permiso conducer, permiso conducción, permiso conducer, permiso de conducción | 
| Sweden |  ajokortin numero, dlno\# ajokortti, drivere lic\., förare lic\., körkort, körkort nummer, körkortsnummer, kuljettajat lic\.  | 
| UK | dl\#, dl:, dlno\#, driver licence, driver licences, driver license, driver licenses, driver permit, drivers lic\., drivers licence, driver's licence, drivers licences, driver's licences, drivers license, driver's license, drivers licenses, driver's licenses, drivers permit, driver's permit, drivers permit number, driving licence, driving license, driving permit | 
| US | dl\#, dl:, dlno\#, driver licence, driver licences, driver license, driver licenses, driver permit, drivers lic\., drivers licence, driver's licence, drivers licences, driver's licences, drivers license, driver's license, drivers licenses, driver's licenses, drivers permit, driver's permit, drivers permit number, driving licence, driving license, driving permit | 

## Keywords for national identification numbers<a name="sns-managed-data-identifiers-pii-natlid-keywords"></a>

To detect various types of national identification numbers, Amazon SNS requires a keyword to be in close proximity to the numbers\. This includes Documento Nacional de Identidad \(DNI\) identifiers \(Spain\), French National Institute for Statistics and Economic Studies \(INSEE\) codes, German National Identity Card numbers, and Registro Geral \(RG\) numbers \(Brazil\)\.

The following table lists the keywords that Amazon SNS recognizes for specific countries and regions\.


| Country or region | Keywords | 
| --- | --- | 
| Brazil | registro geral, rg | 
| France | assurance sociale, carte nationale d’identité, cni, code sécurité sociale, French social security number, fssn\#, insee, insurance number, national id number, nationalid\#, numéro d'assurance, sécurité sociale, sécurité sociale non\., sécurité sociale numéro, social, social security, social security number, socialsecuritynumber, ss\#, ssn, ssn\# | 
| Germany | ausweisnummer, id number, identification number, identity number, insurance number, personal id, personalausweis | 
| Italy | codice fiscal, dati anagrafici, ehic, health card, health insurance card, p\. iva, partita i\.v\.a\., personal data, tax code, tessera sanitaria | 
| Spain | dni, dni\#, dninúmero\#, documento nacional de identidad, identidad único, identidadúnico\#, insurance number, national identification number, national identity, nationalid\#, nationalidno\#, número nacional identidad, personal identification number, personal identity no, unique identity number, uniqueid\# | 

## Keywords for passport numbers<a name="sns-managed-data-identifiers-pii-passport-keywords"></a>

To detect various types of passport numbers, Amazon SNS requires a keyword to be in proximity of the numbers\. The following table lists the keywords that Amazon SNS recognizes for specific countries and regions\.


| Country or region | Keywords | 
| --- | --- | 
| Canada | passeport, passeport\#, passport, passport\#, passportno, passportno\# | 
| France | numéro de passeport, passeport, passeport\#, passeport \#, passeportn °, passeport n °, passeportNon, passeport non | 
| Germany | ausstellungsdatum, ausstellungsort, geburtsdatum, passport, passports, reisepass, reisepass–nr, reisepassnummer | 
| Italy | italian passport number, numéro passeport, numéro passeport italien, passaporto, passaporto italiana, passaporto numero, passport number, repubblica italiana passaporto | 
| Spain | españa pasaporte, libreta pasaporte, número pasaporte, pasaporte, passport, passport book, passport no, passport number, spain passport | 
| UK | passeport \#, passeport n °, passeportNon, passeport non, passeportn °, passport \#, passport no, passport number, passport\#, passportid | 
| US | passport, travel document | 

## Keywords for taxpayer identification and reference numbers<a name="sns-managed-data-identifiers-financial-tin-keywords"></a>

To detect various types of taxpayer identification and reference numbers, Amazon SNS requires a keyword to be in proximity of the numbers\. The following table lists the keywords that Amazon SNS recognizes for specific countries and regions\.


| Country or region | Keywords | 
| --- | --- | 
| Brazil | cadastro de pessoa física, cadastro de pessoa fisica, cadastro de pessoas físicas, cadastro de pessoas fisicas, cadastro nacional da pessoa jurídica, cadastro nacional da pessoa juridica, cnpj, cpf | 
| France | numéro d'identification fiscale, tax id, tax identification number, tax number, tin, tin\# | 
| Germany | identifikationsnummer, steuer id, steueridentifikationsnummer, steuernummer, tax id, tax identification number, tax number | 
| Spain | cif, cif número, cifnúmero\#, nie, nif, número de contribuyente, número de identidad de extranjero, número de identificación fiscal, número de impuesto corporativo, personal tax number, tax id, tax identification number, tax number, tin, tin\# | 
| UK | paye, tax id, tax id no\., tax id number, tax identification, tax identification\#, tax no\., tax number, tax reference, tax\#, taxid\#, temporary reference number, tin, trn, unique tax reference, unique taxpayer reference, utr | 
| US | individual taxpayer identification number, itin, i\.t\.i\.n\. | 

## Data identifier ARNs for personally identifiable information \(PII\)<a name="sns-message-data-protection-pii-arns"></a>

The following table lists the Amazon Resource Names \(ARNs\) for the data identifiers that you can add to your data protection policies\.


| PII data identifier ARNs | 
| --- | 
| arn:aws:dataprotection::aws:data\-identifier/Address | 
| arn:aws:dataprotection::aws:data\-identifier/CepCode\-BR | 
| arn:aws:dataprotection::aws:data\-identifier/Cnpj\-BR | 
| arn:aws:dataprotection::aws:data\-identifier/CpfCode\-BR | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-AT | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-AU | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-BE | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-BG | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-CA | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-CY | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-CZ | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-DE | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-DK | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-EE | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-ES | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-FI | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-FR | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-GB | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-GR | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-HR | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-HU | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-IE | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-IT | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-LT | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-LU | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-LV | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-MT | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-NL | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-PL | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-PT | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-RO | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-SE | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-SI | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-SK | 
| arn:aws:dataprotection::aws:data\-identifier/DriversLicense\-US | 
| arn:aws:dataprotection::aws:data\-identifier/ElectoralRollNumber\-GB | 
| arn:aws:dataprotection::aws:data\-identifier/EmailAddress | 
| arn:aws:dataprotection::aws:data\-identifier/IndividualTaxIdentificationNumber\-US | 
| arn:aws:dataprotection::aws:data\-identifier/InseeCode\-FR | 
| arn:aws:dataprotection::aws:data\-identifier/LatLong | 
| arn:aws:dataprotection::aws:data\-identifier/Name | 
| arn:aws:dataprotection::aws:data\-identifier/NationalIdentificationNumber\-DE | 
| arn:aws:dataprotection::aws:data\-identifier/NationalIdentificationNumber\-ES | 
| arn:aws:dataprotection::aws:data\-identifier/NationalIdentificationNumber\-IT | 
| arn:aws:dataprotection::aws:data\-identifier/NieNumber\-ES | 
| arn:aws:dataprotection::aws:data\-identifier/NifNumber\-ES | 
| arn:aws:dataprotection::aws:data\-identifier/PassportNumber\-CA | 
| arn:aws:dataprotection::aws:data\-identifier/PassportNumber\-DE | 
| arn:aws:dataprotection::aws:data\-identifier/PassportNumber\-ES | 
| arn:aws:dataprotection::aws:data\-identifier/PassportNumber\-FR | 
| arn:aws:dataprotection::aws:data\-identifier/PassportNumber\-GB | 
| arn:aws:dataprotection::aws:data\-identifier/PassportNumber\-IT | 
| arn:aws:dataprotection::aws:data\-identifier/PassportNumber\-US | 
| arn:aws:dataprotection::aws:data\-identifier/PermanentResidenceNumber\-CA | 
| arn:aws:dataprotection::aws:data\-identifier/PhoneNumber | 
| arn:aws:dataprotection::aws:data\-identifier/PhoneNumber\-BR | 
| arn:aws:dataprotection::aws:data\-identifier/PhoneNumber\-DE | 
| arn:aws:dataprotection::aws:data\-identifier/PhoneNumber\-ES | 
| arn:aws:dataprotection::aws:data\-identifier/PhoneNumber\-FR | 
| arn:aws:dataprotection::aws:data\-identifier/PhoneNumber\-GB | 
| arn:aws:dataprotection::aws:data\-identifier/PhoneNumber\-IT | 
| arn:aws:dataprotection::aws:data\-identifier/PhoneNumber\-US | 
| arn:aws:dataprotection::aws:data\-identifier/PostalCode\-CA | 
| arn:aws:dataprotection::aws:data\-identifier/RgNumber\-BR | 
| arn:aws:dataprotection::aws:data\-identifier/SocialInsuranceNumber\-CA | 
| arn:aws:dataprotection::aws:data\-identifier/Ssn\-ES | 
| arn:aws:dataprotection::aws:data\-identifier/Ssn\-US | 
| arn:aws:dataprotection::aws:data\-identifier/TaxId\-DE | 
| arn:aws:dataprotection::aws:data\-identifier/TaxId\-ES | 
| arn:aws:dataprotection::aws:data\-identifier/TaxId\-FR | 
| arn:aws:dataprotection::aws:data\-identifier/TaxId\-GB | 
| arn:aws:dataprotection::aws:data\-identifier/VehicleIdentificationNumber | 
| arn:aws:dataprotection::aws:data\-identifier/ZipCode\-US | 