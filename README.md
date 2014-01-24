THIS IS A WORK IN PROGRESS

mostly just a placeholder at this point.

stay tuned.


## Concept
-------------

###app.js
```javascript
var $ = require('dquery');

$.connect('localhost:27017/test');  // Connect to database

$.collections({
    restaurants: 'restaurant',    // collectionName: documentName
    foods: 'food',
    users: 'user',
    orders: 'order'
});

$.loadDSS('./main.dss')
```
###main.dss
```css
.new {
    collection: 'restaurants';
    filter: {
        created: {
            $gte: Date(Date.now() - 1000*60*60*24*7)
        }
    }
}

order:placedBy($user) {
    filter: {
        user._id: $user._id
    }
}


```

###app.js

```javascript
app.get('/api/favorites', function(req, res){
    
    var _ids = $('user#' + req.session.user._id + ' > favorites > _id');
    
    $('restaurant:byIds($ids)', {$ids: _ids }).page(req.body).then(function(restaurants){
        res.send({
            success: true, 
            data: restaurants
        })
    }).catch(function(e){ 
        res.send({
            success:false, 
            msg: e.message
        }) 
    })


})
```





