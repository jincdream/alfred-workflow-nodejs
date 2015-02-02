Alfred Workflow Nodejs Library
=========

A small library providing helpers to create Alfred Workflow

* Workflow & Item - Helper to build and generate feedbacks
* Storage - Helper to CRUD data
* Settings - Helper to CRUD settings, store password securely
* Utils 

## Installation ##

```
#!bash
npm install "alfred-workflow-nodejs"
```
## Tests ##

```
#!bash
npm test
```
## Usage ##
### Workflow skeleton ###
Workflow command
```
#!bash
/usr/local/bin/node main.js "action" "query"
```
**main.js**
```
#!javascript
var AlfredNode = require('alfred-workflow-nodejs');
var actionHandler = AlfredNode.actionHandler;
var wf = AlfredNode.workflow;
var Item = AlfredNode.Item;

(function main() {
    actionHandler.onAction("action1", function(query) {
        // your code to handle action 1 here
    });
    actionHandler.onAction("action2", function(query) {
        // your code to handle action 2 here
    });

    AlfredNode.run();
})();
```

### Workflow and Item - Generate feedbacks ###
* Workflow is used to build and generate feedbacks

```
#!javascript
var workflow = AlfredNode.workflow;
// set name for workflow (you SHOULD set name for your wf)
workflow.setName("example-alfred-workflow-using-nodejs");
```

* Item is class that prepresent data of a feedback:
    * uid
    * title
    * subtitle
    * arg
    * icon
    * valid(true/false, default is false)
    * autocomplete

```
#!javascript
var Item = AlfredNode.Item;
var item1 = new Item({title: "item 1", subtile: "sub 1"});
var item2 = new Item({uid: "uid", title: "item 1", subtile: "sub 1", valid: true, icon: "icon.png", arg: "arg",  autocomplete: "autocomplete"});
workflow.addItem(item1);
workflow.addItem(item2);
// generate feedbacks
workflow.feedback();

```

### Storage - APIs to CRUD data ###
* set(key, value, [ttl])
    * key: string
    * value: object
    * ttl: long (milisecond)
* get(key)
* remove(key)
* clear() : clear all data, be carefull!!!

```
#!javascript
var storage = AlfredNode.storage;
storge.set("key", "value");
storage.set("key", {"name":"node", 1000}; //ttl in miliseconds
storage.get("key");
storage.remove("key");
storage.clear();
```
    
### Settings - APIs to CRUD settings ###
Helpers to store string key/value settings, store password into Mac keychain

* set(key, value, [ttl])
    * key: string
    * value: string
* get(key)
* remove(key)
* clear() : clear all settings, be carefull!!!
* setPassword(username, password) : store password to Mac keychain (workflow name is used here as keychain service)
* getPassword(username, callback(error,password)) : get password of username from Mac keychain
    * username
    * callback(error, password): callback function that is called after password is returned

```
#!javascript
var settings = AlfredNode.settings;
settings.set("key", "stringValue");
settings.get("key");
settings.remove("key");
settings.clear(); //clear all settings!!!
settings.setPassword("username", "password"); // store passwork into keychain
// get password from settings, async function
settings.getPassword("username", function(error, password){
    console.log(password);
});
```
  
### Utils - Helper functions ###
Some utilities

* filter(query, list, keyBuilder) : filter list of object using fuzzy matching
    * query
    * list
    * keyBuilder : function to build key to compare from items in list
    
```
#!javascript
var utils = AlfredNode.utils;
// filter array of string/object using fuzzy matching
utils.filter("a", ["a", "b", "c"], function(item){return item});
// => return ["a"]
utils.filter("pen", [{name: "pencil"}, {name: "pen"}, {name: "book"}], function(item){ return item.name});
// => return [{name: "pencil"}, {name: "pen"}]
```

### Notes ###
You can look at some tests in test folder in source code get more about usage

## Source code and document ##
https://github.com/giangvo/alfred-workflow-nodejs
or 
https://bitbucket.org/giangvo_Atlassian/alfred-workflow-nodejs

## Release History ##
* 0.0.1 Initial release
* 0.0.2 Fix bug storage
* 0.0.3 Add workflow skeleton
* 0.0.4 Add more docs and update git repo