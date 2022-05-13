export default {
 async fetch(request, environment) {
   const url = new URL(request.url);
   switch (url.pathname) {
     case '/login':
       return await environment.login.fetch(request);

     case '/logout':
       return await environment.logout.fetch(request);

     case '/admin': {
       // Check that the "Authorization" header is sent when authenticated.
       const authCheck = await environment.auth.fetch(request.clone());
       if (authCheck.status != 200) { return authCheck }
       // If the auth check passes, send the request to the /admin endpoint
       return await environment.admin.fetch(request);
     }

     case '/robots.txt':
       return new Response(null, { status: 204 });
   }
   return new Response('Not Found.', { status: 404 });
 }
}```
