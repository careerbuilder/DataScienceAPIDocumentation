
·       Accepts GET and POST requests  
·       Accepts title, desc, and reqs parameters (must provide at least one)  
·       Returns a JSON response data with a single contains_spam element (1.0 for true, 0.0 for false)
 
An example request: https://wwwtest.api.careerbuilder.com/core/tagging/spamdetector?title=engineer&desc=solve%20hard%20problems&reqs=four%20year%20degree
 
And its response:

    {
      "data": {
        "contains_spam": "0.0"
      }
    }

#Versioning
-----------
The current version is 1.0. The data returned is versioned.

Our general versioning strategy is available [here](/Versioning.md).
