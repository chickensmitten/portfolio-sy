reactstrap and bootstrap version

`npm install --save reactstrap@8.9.0 bootstrap@4.4.1`

if using scss, have to npm install --save sass

```
npm install swr

npm install --save dotenv-webpack@1.7.0

npm install --save @auth0/nextjs-auth0@0.11.0

npm install react-hook-form@5.7.2 --save

npm install react-datepicker --save

npm install react-toastify@6.0.4
```

with .env.procution.local, when you run `npm run build`, it will build your app from that file.

# Adding Roles to Auth0
```
function (user, context, callback) {
   const NAMESPACE = 'http://portfolio-jerga.com';
   user.app_metadata = user.app_metadata || {};
   // You can add a Role based on what you want
   // In this case I check domain
   var addRolesToUser = function(user, cb) {
     if (user.email === 'explanation@gmail.com') {
       cb(null, ['admin']);
     } else {
       cb(null, ['guest']);
     }
   };

   addRolesToUser(user, function(err, roles) {
     if (err) {
       callback(err);
     } else {
       user.app_metadata.roles = roles;
       auth0.users.updateAppMetadata(user.user_id, user.app_metadata)
         .then(function(){
           context.idToken[NAMESPACE + '/roles'] = user.app_metadata.roles;
           context.accessToken[NAMESPACE + '/roles'] = user.app_metadata.roles;
           callback(null, user, context);
         })
         .catch(function(err){
           callback(err);
         });
     }
   });
 }
```