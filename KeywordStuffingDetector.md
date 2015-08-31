I am pleased to share with you the Data Science Team’s latest release – the keyword stuffing detector (AKA spam detector), a classifier for identifying spam in job postings.
 
This service is currently available in test and will be up in production soon. Using the normal OAuth handshake, you can access this service at https://wwwtest.api.careerbuilder.com/core/tagging/spamdetector.
 
Here are some facts about this service:
·       Hot and fresh out the kitchen
·       Accepts GET and POST requests
·       Accepts title, desc, and reqs parameters (must provide at least one)
·       Returns a JSON response data with a single contains_spam element (1.0 for true, 0.0 for false)
·       Developed and trained by Yun Zhu, former R&D team intern who will be starting with us full-time in January (so thank him if you see him, he did the hard part!)
 
An example request: https://wwwtest.api.careerbuilder.com/core/tagging/spamdetector?title=engineer&desc=solve%20hard%20problems&reqs=four%20year%20degree
 
And its response:

    {
      "data": {
        "contains_spam": "0.0"
      }
    }
 
As a final note – we’ve observed some slowness in the classifier while parsing larger texts. This is currently being investigated.

#Versioning
-----------
The current version is 1.0. The data returned is versioned.

Our general versioning strategy is available [here](/Versioning.md).

 
Please let me or the Data Science Application Development team know if you have any questions.
