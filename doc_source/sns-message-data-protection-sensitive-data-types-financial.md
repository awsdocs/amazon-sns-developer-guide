# Sensitive data types: Financial<a name="sns-message-data-protection-sensitive-data-types-financial"></a>

The following table lists and describes the types of financial information that Amazon SNS can detect using managed data identifiers\.


| Detection type | Managed data identifier ID | Keyword required | Additional information | Countries and regions | 
| --- | --- | --- | --- | --- | 
|  Bank account number  |  BankAccountNumber BankAccountNumber\-US  |  Yes, see [Keywords for bank account numbers](#sns-managed-data-identifiers-bank-keywords)\.  |  This includes: International Bank Account Numbers \(IBANs\) that consist of up to 34 alphanumeric characters, including elements such as country code\.  |  France, Germany, Italy, Spain, UK  | 
|  Credit card expiration date  |  CreditCardExpiration  |  exp d, exp m, exp y, expiration, expiry  |  –  |  Any  | 
| Credit card magnetic strip data | CreditCardMagneticStripe | Yes, including: card data, iso7813, mag, magstripe, stripe, swipe\. | This includes tracks 1 and 2\. | Any | 
|  Credit card number  |  CreditCardNumber  |  account number, american express, amex, bank card, card, card num, card number, cc \#, ccn, check card, credit, credit card\#, dankort, debit, debit card, diners club, discover, electron, elo verification code, japanese card bureau, jcb, mastercard, mc, pan, payment account number, payment card number, pcn, union pay, visa  |  Detection requires the data to be a 13–19 digit sequence that adheres to the Luhn check formula, and uses a standard card number prefix for any of the following types of credit cards: American Express, Dankort, Diner’s Club, Discover, Electron, Japanese Card Bureau \(JCB\), Mastercard, UnionPay, and Visa \(superscript link below 1\)\.  |  Any  | 
|  Credit card verification code  |  CreditCardSecurityCode  |  card id, card identification code, card identification number, card security code, card validation code, card validation number, card verification data, card verification value, cvc, cvc2, cvv, cvv2, elo verification code  |  –  |  Any  | 

1. <a name="sns-managed-data-identifiers-cc-exceptions"></a>Amazon SNS doesn't report occurrences of the following sequences, which credit card issuers have reserved for public testing:

   122000000000003, 2222405343248877, 2222990905257051, 2223007648726984, 2223577120017656, 30569309025904, 34343434343434, 3528000700000000, 3530111333300000, 3566002020360505, 36148900647913, 36700102000000, 371449635398431, 378282246310005, 378734493671000, 38520000023237, 4012888888881881, 4111111111111111, 4222222222222, 4444333322221111, 4462030000000000, 4484070000000000, 4911830000000, 4917300800000000, 4917610000000000, 4917610000000000003, 5019717010103742, 5105105105105100, 5111010030175156, 5185540810000019, 5200828282828210, 5204230080000017, 5204740009900014, 5420923878724339, 5454545454545454, 5455330760000018, 5506900490000436, 5506900490000444, 5506900510000234, 5506920809243667, 5506922400634930, 5506927427317625, 5553042241984105, 5555553753048194, 5555555555554444, 5610591081018250, 6011000990139424, 6011000400000000, 6011111111111117, 630490017740292441, 630495060000000000, 6331101999990016, 6759649826438453, 6799990100000000019, and 76009244561\.

## Keywords for bank account numbers<a name="sns-managed-data-identifiers-bank-keywords"></a>

Use the following keywords to detect International Bank Account Numbers \(IBANs\) that consist of up to 34 alphanumeric characters, including elements such as country code\.


| Country or region | Keywords | 
| --- | --- | 
|  France  |  account code, account number, accountno\#, accountnumber\#, bban, code bancaire, compte bancaire, customer account id, customer account number, customer bank account id, iban, numéro de compte  | 
|  Germany  |  account code, account number, accountno\#, accountnumber\#, bankleitzahl, bban, customer account id, customer account number, customer bank account id, geheimzahl, iban, kartennummer, kontonummer, kreditkartennummer, sepa  | 
|  Italy  |  account code, account number, accountno\#, accountnumber\#, bban, codice bancario, conto bancario, customer account id, customer account number, customer bank account id, iban, numero di conto  | 
|  Spain  |  account code, account number, accountno\#, accountnumber\#, bban, código cuenta, código cuenta bancaria, cuenta cliente id, customer account ID, customer account number, customer bank account id, iban, número cuenta bancaria cliente, número cuenta cliente  | 
|  UK  |  account code, account number, accountno\#, accountnumber\#, bban, customer account id, customer account number, customer bank account id, iban, sepa  | 
| US | bank account, bank acct, checking account, checking acct, deposit account, deposit acct, savings account, savings acct, chequing account, chequing acct | 

### Data identifier ARNs for financial data types<a name="sns-message-data-protection-financial-arns"></a>

The following lists the Amazon Resource Names \(ARNs\) for the data identifiers that you can add to your data protection policies\.


| Financial data identifier ARNs | 
| --- | 
| arn:aws:dataprotection::aws:data\-identifier/BankAccountNumber\-DE | 
| arn:aws:dataprotection::aws:data\-identifier/BankAccountNumber\-ES | 
| arn:aws:dataprotection::aws:data\-identifier/BankAccountNumber\-FR | 
| arn:aws:dataprotection::aws:data\-identifier/BankAccountNumber\-GB | 
| arn:aws:dataprotection::aws:data\-identifier/BankAccountNumber\-IT | 
| arn:aws:dataprotection::aws:data\-identifier/BankAccountNumber\-US | 
| arn:aws:dataprotection::aws:data\-identifier/CreditCardExpiration | 
| arn:aws:dataprotection::aws:data\-identifier/CreditCardNumber | 
| arn:aws:dataprotection::aws:data\-identifier/CreditCardSecurityCode | 