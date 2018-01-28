# serverless-aws-js
Web page to manage AWS resources in your browser

From the command (if you have the AWS CLI package installed), starting is as easy as e.g.
```aws ec2 start-instances --instance-ids i-00ba4df9e1dbe0eec```

Which you could put in a .bat or powershell script (along with setting up credentials), 
and run from desktop, scheduled task, or IIS.

But this requires installing a fairly large package, everywhere it is to be called from -- or else as a server script.

Alternative:  call it directly from your browser using the javascript AWS SDK.
Serverless and zero-install.

You can save this html locally and run it -- no server required, your browser calls AWS directly.  
Or serve the page through an S3 bucket, share from dropbox, or execute directly 
from
https://toddkaufmann.github.io/serverless-aws-js/manage-instances.html
3
Knowledge of AWS Keys is required, though.
After you put in key, secret key, and region,  click 'refresh instance status' to list instances.
You can also start & stop instances from here.
Bug (fixable):  after initially entering region you can't change it.

This proof-of-concept is all plain vanilla javascript, no other libraries besides the aws-sdk, and could be made much nicer with some css, jquery etc.,
and require less lines of code (but more libraries) (but my javascript was rusty and needed some exercise).
It performs some basic AWS EC2 console functions without requiring login to their console.
It can easily be customized to do 'more', such as
listing instances from multiple regions, or accounts
filtering list by type, tag, AMI, status etc.
show more/other detail (IP, DNS, volumes attached, etc)
display other resources: VPCs, network interfaces, RDS, etc
basically:  custom AWS dashboard..  resource prices could be pulled in too.
safety feature ("are you sure?"), other operations (terminate, clone, bid for more like these on spot market,  etc)
automatic refresh.
Note all AWS operations are asynchronous:  the call to 'start-instance' returns immediately, and to find when the system is actually up and usable you need to poll or have it send an event.
Other  use cases:  What Is the AWS SDK for JavaScript? - AWS SDK for JavaScript
Creating new instances are about the same, but more information needs to be supplied:  AMI, machine type, VPC, security group, user data etc.
Anything available from the (Ec2) web console is possible and more 
(though they hide a lot of the complexity; some single 'clicks' translate to multiple API calls).

Once the API is understood, it's mostly the same calls from command line, javascript, python, php, etc., with same json output.
Dealing with this and exceptions may differ / be easier / more convenient libraries or readymade scripts available.

SECURITY:
if all your users can handle the keys, or access this page (if you put it on your server), then you could even hardcode the info in the page
(you can always change/revoke them in your account).  I believe an associated policy could also allow operations using this key to only come from a particular network (eg, bxvideo).
If AWS calls are to be made directly from the browser, using Cognito is the recommended way; see Setting Credentials in a Web Browser - AWS SDK for JavaScript.
But, that's some extra steps, I just wanted to get the javascript working.

Since you (can) have a server, this is all easily moved to server side with node (no code changes), or lambda, which will prevent user access to credentials so they are never in the user's browser.  You will need to control access some other way then (eg intranet only).
