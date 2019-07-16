# Sharepoint 2013/2016/2019/Online, Office 365 REST API Code Sample/Example
In this repo contains Sharepoint 2013/2016/2019/Online, Office 365 REST API Code Sample/Example which will using SP Rest utility `SPRest.ts` or `SPRest.js`
Here utility library can be used with TypeScript in #Spfx and also work with most browsers.

As I have used `fetch` API which is not available in IE11 browser so you can use [polyfill](https://github.com/github/fetch)

## Sharepoint 2013/2016/2019/Online List Item CRUD Operation

```js
var spRest=new SPRest("https://brgrp.sharepoint.com");

// Get All Item from List item  
spRest.Utils.ListItem.GetAllItem({listName:"PlaceHolderList"}).then(function(r){  
console.log(r);  
// Response received. TODO bind record to table or somewhere else.  
});

//Get all selected column Data using full URL  
util.Utils.ListItem  
.GetAllItem({  
   url:"https://brgrp.sharepoint.com/_api/web/lists/getbytitle('PlaceHolderList')/items?$select=Id,Title&$top=200"  
}).then(function(r){console.log(r);  
//Response Received  
});  
  
//Get all selected column data with listName and oDataOption  
  
util.Utils.ListItem.GetAllItem(  
   {"listName":"PlaceHolderList"  
      ,oDataOption:"$select=Id,Title&$top=200"  
   }).then(function(r){console.log(r);  
   //Response Received  
});  

// Get List Item By Id  
spRest.Utils.ListItem.GetItemById({listName:"PlaceHolderList",Id:201}).then(function(r){    
console.log(r);    
// Response received.   
});

// Add ListItem to Sharepoint List  
spRest.Utils.ListItem.Add({listName:"PlaceHolderList"
,data:{Title:"New Item Created For Demo",UserId:1,Completed:"true"}}).then(function(r){    
console.log(r);    
// Added New List item response received with newly created item  
}); 

// Update List item based on ID with new data in SharePoint List  
spRest.Utils.ListItem.Update({listName:"PlaceHolderList",Id:201
,data:{Title:"Updated List Item",UserId:1,Completed:"true"}}).then(function(r){    
// List Item Updated and received response with status 204  
console.log(r);  
}); 

// Delete List item based on ID  
spRest.Utils.ListItem.Delete({listName:"PlaceHolderList",Id:201}).then(function(r){    
// List Item Deleted and received response with status 200  
console.log(r);  
}); 
```

## Upload or Create txt file in SharePoint Document Library

```js
var reqUrl="https://brgrp.sharepoint.com/_api/web/GetFolderByServerRelativeUrl('/Shared Documents')"
fetch(reqUrl+"/Files/add(url='file_name.txt',overwrite=true)",
{method:"POST",headers:
{accept:"application/json;odata=verbose",
"Content-Type":"application/json;odata=verbose","X-RequestDigest":temp2}
,body:"Content Of Text File"}).then(r=>console.log(r))
```

## Update A SharePoint List Item Without Increasing Its Item File Version Using Rest API


```js
//payload for request   
 body=  {"formValues":[{"FieldName":"Title","FieldValue":"Single Update Title with versioning__"}],bNewDocumentUpdate:true}  
  
 //Header data for sharepoint POST Request  
 var _payloadOptions = {  
                method: "POST",  
                body: undefined,  
                headers: {  
                    credentials: "include",  
                    Accept: "application/json; odata=verbose",  
                    "Content-Type": "application/json; odata=verbose"  
                }  
            };  
  
//Get RequestDigest First  
fetch("https://brgrp.sharepoint.com/_api/contextinfo",_payloadOptions).then(r=>r.json())  
.then(r=>  
                                   {  
_payloadOptions.headers["X-RequestDigest"]=r.d.GetContextWebInformation.FormDigestValue  
      
_payloadOptions.body=JSON.stringify(body);  
  
// Make REST API Call to update list item without increamenting version.  
fetch("https://brgrp.sharepoint.com/_api/web/Lists/GetbyTitle('Documents')/items(1)/ValidateUpdateListItem()"
,_payloadOptions).then(r=>r.json()).then(r=>console.log(r))  
});

```

Reference link : https://www.c-sharpcorner.com/article/update-a-sharepoint-list-item-without-increasing-its-item-file-version-using-res/

