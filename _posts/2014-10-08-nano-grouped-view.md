---
layout: post
title: Getting a grouped view using nano
---

The [nano](https://github.com/dscape/nano) package for node.js provides a clean and easy API for accessing a CouchDB database. This makes it an ideal technology when building a node.js app with a Cloudant database in [Bluemix](www.bluemix.net).

When doing this I needed to call a view and pass the `group=true` parameter to get the reduced values needed for display. The code snippet below shows how this was achieved:

``` js
db.view("designName", "viewName", {group:"true"}, function(err, body){
	if(!err){
		//process the response in body
	}
})
```
