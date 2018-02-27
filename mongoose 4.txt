var util = require('util');
var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/test');

var db = mongoose.connection;
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', function() {
 console.log("connected to mongo");
});

//creating schema to save the document

var schema = new mongoose.Schema({ name: 'string', balance: 'string' });
var userBalance = mongoose.model('Account', schema);

var bodyParser = require('body-parser');

var cache = require('memory-cache');
var arr = [];

module.exports = {
  getPosts(req, res) {
	res.send(util.inspect(userBalance.find()));
	console.log((util.inspect(userBalance.find())));
  },
  addPost(req, res) {
	var data = '';
	req.on('data', function( chunk ) {
    	data += chunk;
  	});
  	req.on('end', function() {
	req.rawBody = data;
    
        if (data && data.indexOf('{') > -1 ) {
	req.body = JSON.parse(data);
	//arr.push(data);
	//cache.put("posts",arr);	
	var small = new userBalance(req.body);
	small.save(function (err) {
	  if (err) return handleError(err);
	  // saved!
	})	
    }
  });

	res.send("post created successfully");
  },
  updatePost(req, res) {
	//updating the post details
	var data = '';
	req.on('data', function( chunk ) {
    	data += chunk;
  	});
  	req.on('end', function() {
	req.rawBody = data;
    
        if (data && data.indexOf('{') > -1 ) {
	req.body = JSON.parse(data);	
	//var arrData = cache.get("posts");	
	//arrData.pop(req.params.id);
	//arrData.push(data);
	//cache.put("posts",arrData);
	
    }
  });	
	res.send("post udpated successfully");
  },
  removePost(req, res) {
	//var arrData = cache.get("posts");	
	//arrData.pop(req.params.id);
	//cache.put("posts",arrData);
	
	res.send("post deludpatedccessfully");
  }
}
console.log("posts");