# Troubleshooting SMS messages sent to recipients in India<a name="sns-send-sms-india-troubleshooting"></a>

The following are some reasons carriers may block SMS messages:
+ **No template was found that matched the content sent\.**

  Content sent: **<\#> 12345 is your OTP to verify mobile number\. Your OTP is valid for 15 minutes \-\- ABC Pvt\. Ltd\.**

  Matched template: None

  Issue: There are no DLT templates that include `<#>` or `{#var#}` at the beginning of the DLT registered template\.
+ **The value of a variable exceeds 30 characters\.**

  Content sent: **12345 is your OTP code for ABC \(ABC Company \- India Private Limited\) \- \(ABC 123456789\)\. Share with your agent only\. \- ABC Pvt\. Ltd\.**

  Matched template: **\{\#var\#\} is your OTP code for \{\#var\#\} \(\{\#var\#\}\) \- \(\{\#var\#\} \{\#var\#\}\)\. Share with your agent only\. \- ABC Pvt\. Ltd\.**

  Issue: The value of “ABC Company \- India Private Limited” in the content sent exceeds a single `{#var#}` character limit of 30\. 
+ **The message sentence case does not match the sentence case in the template\.**

  Content sent: **12345 is your OTP code for ABC \(ABC Company \- India Private Limited\) \- \(ABC 123456789\)\. Share with your agent only\. \- ABC Pvt\. Ltd\.**

  Matched template: **\{\#var\#\} is your OTP code for \{\#var\#\} \(\{\#var\#\}\) \- \(\{\#var\#\} \{\#var\#\}\)\. Share with your agent only\. \- ABC PVT\. LTD\.**

  Issue: The company name appended to the DLT matched template is capitalized while the content sent has changed parts of the name to lowercase — “ABC Pvt\. Ltd\.” vs\. “ABC PVT\. LTD\.” 