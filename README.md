angular-cloud-kit
=================

An angularjs serviceprovider to utilize CloudKit with angular

## Installation

For installation the use of Bower is recommended.

### Bower
Call the following command on your command line:

```sh
bower install --save angular-cloud-kit
```

And add the following line to your html file, for example `index.html`:

```html
<script src="components/angular-cloud-kit/angular-cloud-kit.js"></script>
```


### Manual

- Download file.
- Add the following line to your html file:

```html
<script src="angular-cloud-kit.js"></script>
```

## Usage

Normally, and as a recommendation, you have only one indexedDB per app.
Thus in your `app.js` where you define your module, you do:

```javascript
angular.module('myModuleName', ['ngCloudKit'])
  .config(function ($cloudKitProvider) {
    $cloudKitProvider
      .container('com.example.apple-samplecode.cloudkit-catalog')
      .apitoken('<Insert token Token Here>')
  });
```
The connection method takes the databasename as parameter,
the upgradeCallback has 3 parameters:
function callback(event, database, transaction). For upgrading your db structure, see 
https://developer.mozilla.org/en-US/docs/IndexedDB/Using_IndexedDB.

You can also define your own error handlers, overwriting the default ones, which log to console.


Inside your controller you use `$cloudKit` like this:

```javascript
angular.module('myModuleName')
  .controller('myControllerName', function($scope, $cloudKit) {
    
    $scope.objects = [];
    
    var OBJECT_STORE_NAME = 'people';  
        
    /**
     * @type {ObjectStore}
     */
    var myObjectStore = $cloudKit.objectStore(OBJECT_STORE_NAME);
    
    myObjectStore.insert({"ssn": "444-444-222-111","name": "John Doe", "age": 57}).then(function(e){...});
    
    myObjectStore.getAll().then(function(results) {  
      // Update scope
      $scope.objects = results;
    });

  /**
   * execute a query:
   * presuming we've an index on 'age' field called 'age_idx'
   * find all persons older than 40 years
   */
   
   var myQuery = $cloudKit.queryBuilder().$index('age_idx').$gt(40).$asc.compile();
   myObjectStore.each(myQuery).then(function(cursor){
     cursor.key;
     cursor.value;
     ...
   });
  });
```

QueryBuilder aka IDBKeyRange maybe needs some revision.
This is all the info you get for now, for more read the code, it's ndoc-annotated! 

Important note: this software is in alpha state and therefore it's used at your own risk,
don't make me liable for any damages or loss of data!

Status Update: I'm sorry to say that I've abandoned angularJS for now and therefore have no plans for features or updates. This may change in the future. Apart from that I will apply updates submitted as pull requests when I find the time and see use.

Anyone willing to join as active developer is welcome to drop me a note and help keep this project alive.

