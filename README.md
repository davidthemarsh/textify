textify
=======

Create text versions of images based on different libraries.

# Breakdown


Textify is broken up into two pieces:
 
 * The url contract
 * Widgets that interact with them
 
# URL Contract


The URL contract is defined as follows.  All URLs are given assuming that textify is at the root of the domain but all packages support mounting them in other locations.


## Root URL (/) Structure

The root url provides the following end points:
 
 * **providers** - This url returns a JSON object formatted like so:
 
```
{
        providers: ['provider1', 'provider2']
}
```

Where each entry in the provider list is another url that can be accessed by /api/**provider1**

## Provider (/api/*provider*) Structure


Each provider will provide the following end points:

### **/info**

Provides a json object that provides details and information about this provider.  The response has the following format:

```
{
        library: {
                         verison:"whatever",
                         name:   "thing-thing-thinger",
                 },
                 
        parameters: {
                         restrictedContent: {
                                                  type="restricted",
                                                  options=['option1', 'option2', 'option2'],
                                                  required=true
                                            },
                         textContent:       {
                                                type: "text",
                                                required=false
                                            },    
                         rangedContent:     {
                                                  type: "range",
                                                  content: "numerical"
                                                  min: 0,
                                                  max: 100,
                                                  required=true
                                            }
        
                    }
}
```
  
  The library enrtry gives general information about the provider being used to textify the selection.  
  
  The parameters entry gives information about the parameters and accepted input.  Each entry in the object represents the parameter name.  The *type* variable indicates the type of input and what information is available. The following types are supported:
  
  * **restricted** - Represents an input where the only accepted inputs are given in the "options" list. Straying from this list may result in an error.
  *  **text** - This parameter may be free form text.
  *  **range** - This input represents input that must be contained to a range.  There will be three entries available: min, max and content.  Content will define the type of range while the min and max will describe the minimum and maximum accepted entries.  Content may be one of the following items:
      * **numerical** - numbers
      * **alphabetical** - letters.  No digits, punctuation or other characters.

### **/**
Accepts all parameters described by the **info** end point.  The response will be given in the following format:

```
{
    request:{
                  param1:value_given,
                  ...
                  paramN:value_given
            },
    status: "successful",
    errors: {
                  param1:"stuff happened"
            },
    results: "<(^.^<)"
}
```

 * **request** - A summary of the request you made to produce the results
 * **status** - overall status of the request.  Possible entries are 'successful' or 'error'
 * **errors** - A map of error codes and the associated message.  Any error code matching a request parameter is a message about that parameter and why it is an error.
 * **results** Textual results about the image in question.
