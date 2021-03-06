# For What Can I Use This?

The very first step is to create your own request context decorator component. You can see in the diagram below of the ColdBox request context design pattern.

![](https://github.com/ortus/coldbox-platform-documentation/tree/24d3f3d16693b36ca41bf5ce0329c6ff33316ef0/images/RequestContextDecorator.png)

Create a component that extends `coldbox.system.web.context.RequestContextDecorator`, this is to provide all the functionality of an original request context decorator as per the design pattern. Once you have done this, you will create a configure method that you can use for custom configuration when the request context gets created by the framework. Then it’s up to you to add your own methods or override the original request context methods.

In order to access the original request context object you will use the provided method called: `getRequestContext()`.

## Declaration

In the sample below, you can see a custom request context declaration and a configure method. In this example, the configure method will trim all simple values that are found in the request collection.

```javascript
<cffunction name="getValue" returntype="Any" access="Public" output="false">
    <cfargument name="name"         type="string"     required="true">
    <cfargument name="defaultValue" type="any"         required="false" default="NONE">
    <---************************************************************** --->
    <cfscript>
        var originalValue = "";

        //Check if the value exists via original object, else return blank
        if( getRequestContext().valueExists(arguments.name) ){
            originalValue = getRequestContext().getValue(argumentCollection=arguments);
            //check if simple
            if( isSimpleValue(originalValue) ){
                originalValue = trim(originalValue);
            }
        }

        return originalValue;
    </cfscript>
</cffunction>
```

As you can see from the code above, the possibilities to change behavior are endless. It is up to your specific requirements and it’s easy!

### Controller Calling

The request context decorator receives a reference to the ColdBox `controller` object and it can be retrieved by using the `getController()` method. This will give you the ability to call plugins, settings, interceptions and much more, right from your decorator object.

