#Ember getting started

##Ember Training and Learnings

###Important links:
* This page is summary of video - http://campus.codeschool.com/courses/try-ember/level/1/section/1/video/1
* http://emberjs.com/

###Installing Ember:
1. Install https://nodejs.org/en/download/stable/ for windows
2. Install https://git-scm.com/downloads 
3. Make sure to select to use the git path, if it doesnt give you the option go set the path yourself by using
	C:\Users\{user}\AppData\Local\GitHub\PortableGit_{guid}\cmd
4. Run `npm install -g ember-cli` in your command prompt (This will install the ember framework)

###Creating New Ember ProjectL
1. In your command prompt run `path>ember new {project name}`
![image](https://cloud.githubusercontent.com/assets/17876815/13810352/b1a5e670-eb6f-11e5-86ff-3f18bd7fcc69.png)
2. Via command prompt go to the project - `path>cd  projectname`
3. Run `path\project>ember serve`
  This will start up a development server with live reload.
  
  ![image](https://cloud.githubusercontent.com/assets/17876815/13811555/ceb81aec-eb76-11e5-87ee-34d72ae912f3.png)
	
####Possible issue:
_Failed to execute "git ls-remote --tags --heads git://github.com/ember-cli/ember-cli-shims.git", exit code of #128
fatal: unable to connect to github.com_ 
* To solve this run:
	`git config --global url."https://".insteadOf git://`

###Restoring an Ember project from Git clone
On occasion you might want to clone your project to a seperate machine and continue development from there (e.g. Sepperation between Office and Home).

Because gitignore will exclude node_modules and the bower_components, `ember serve` will throw an error complaining about modules being missing or empty.

To get your project back to a working state run the following on `path\projectName>`:

1. npm install
2. npm install -g bower
3. bower install

Now when running `ember serve` everything should be back in order.

###First Overview of App

####Templates
Template will tell Ember what html to render for each page.

`app/templates/application.hbs` is the default template, if index.hbs is added with something in it, the `{{outlet}}` will display what is in there (default empty).

Creating another template will not automatically display. For this we need to tell ember in the router.js file where to go.

To link to a different landing page for a user:
`{{#link-to 'landingPage'}}landingPage{{/link-to}}` 

####Router
Manages the application state, will map the state into the url where user is and need to go to.

app/router.js

```javascript

Router.map(function() {
    ---Application endpoints mapped here---
});

Router.map(function() {
    this.route('orders', {path: '/orders'});
});

```

___OR___

```javascript

Router.map(function() {
    this.route('orders');
});

```

(path will be inferred)

![image](https://cloud.githubusercontent.com/assets/17876815/13815628/a570b224-eb8b-11e5-8cf7-23d70b6b9fc8.png)

(based on template created orders.hbs)

####Routes
Collecting and organising data and handing off the data to templates to be rendered.

Ember CLI provides generator for creating a route and updating the router.

`ember generate route <route-name>`

![image](https://cloud.githubusercontent.com/assets/17876815/13903700/eda41498-ee86-11e5-81cf-f6f3c2a96a09.png)

As you can see it will automatically generate:
* file in routes for you (newroute.js)
* template file (newroute.hbs)

This will also add the route `this.route('newroute');` in the router.js

___Routes extended___

The js file in the routes folder `newroute.js` will extend the ember route and can be customised for this 
particular route without effecting any other route.

_Customizing the Route_

To customise the route you can add a function or hook, in this case a model function will be added which can return anything
to be available to the template.

In my case I added a model hook with returning an array to the template:

app\routes\newroute.js - 

```javascript

export default Ember.Route.extend({
    model() {
        return [{ id:'myid', name:'myname' },
        { id:'myid2', name:'myname2' }
        ];
    }
});

```

and in the app\templates\newroute.hbs - 

```javascript
New route <br>

{{#each model as |name|}}
    id - {{name.id}}<br>
    name - {{name.name}}<br>
{{/each}}

```

This will give me the following outcome:

![image](https://cloud.githubusercontent.com/assets/17876815/13903882/e001aed0-ee8c-11e5-88a5-c0abf025a1de.png)

___Define Dynamic Segments in Router___

First we need to map where the dynamic segement will route. In the router map section we add: `this.route('newroutes', {path: '/newroute/:newroute_id'});`.
The `:` in the path represents the dynamic segment and where it starts, these dynamic segments is placeholders for when you pass your 'id' or other params.
For instance if I have a request coming in for /newroute/1, the browser will go through mapping to try and find a match.

```javascript

Router.map(function() {
  this.route('orders');
  this.route('newroute');
  this.route('newroutes', {path: '/newroute/:newroute_id'}); `-finds match here for /newroute/1 and will then navigate to newroutes template`
});

``` 

Once it macthes the router will pickup on the dynamic value and capture that portion - in this case ':newroute_id' matcheds the '1'.
It will then map the 1 to a newroute_id variable and put it into a hash {newroute_id:1}, which is then sent to the route to tell it what the user request is.

Whatever dynamic data is found will be handed over to the route and can be passed into the model hook to find the data the user has requested. This is usually represented by `params`as can be seen in code snippet bewow:

```javascript

export default Ember.Route.extend({
    model(params) {
        return [{ id:'1', name:'myname' },
        { id:'2', name:'myname2' }
        ].findBy('id', params.newroute_id);
    }
});

```
___NOTE___ in te snippet above we also find a specific record by Id and say that we want this id to be what ever was passed through
 as params, in this case newroute_id was mapped as 1. This will return the record with the id of 1.
 
 * Take note of the following
 
    * Each template will need its own routes js file containing the data that need to be manipulated. So for our template newroutes.hbs we need in our app/routes
     another js file called newroutes which will have the code as mentioned above. In the case of this example we have a newroute.js and a newroutes.js file, both which contains the model hook. The only difference is,
     because my newroutes.hbs only need to display information we requested, the model in the js file in routes will contain params and a findBy.
     
```javascript

`newroute.js`
export default Ember.Route.extend({
    model() {
        return [{ id:'1', name:'myname' },
        { id:'2', name:'myname2' }
        ];
    }
});

`newroutes.js`
export default Ember.Route.extend({
    model(params) {
        return [{ id:'1', name:'myname' },
        { id:'2', name:'myname2' }
        ].findBy('id', params.newroute_id);
    }
});

```

After the data is manipulated to represent what the user has requested, we need a template that can display this data to the user. In this case a simple newroutes.hbs template is created to display what was requested:

```html

this is from new route <br>
id = {{model.id}}
name = {{model.name}}

```   

Outcome after typing newroute/1:

![image](https://cloud.githubusercontent.com/assets/17876815/13919672/309dff78-ef6f-11e5-9ce3-5ee4a42645e4.png)

We can add a link to for each name or id for our data displayed to the user in our template file for newroute. This will allow the user to click on displayed data and be routed to data that they want to see instead of
typing the path to navigate to.

```html

{{#each model as |name|}}
    {{#link-to "newroutes" name}} `Note that next to link-to there are extra paramaters - "newroutes"=route name and name=object to view`
        id - {{name.id}}
    {{/link-to}}
        ::: name - {{name.name}}<br>    
{{/each}}

```

___Nested Route___

Because we do not want to lose the list that will navigate us to the users requested information (we want this list of names on the same page in order to navigate around).

As the current templates are sibling of one another they will end up replacing one another as we navigate and fill the `{{outcome}}` on the main page. This means that when we navigate to newroutes it will replace newroute and
we loose the list of items from our page. In oreder to not loose this listing we use nested routes, which will allow us to display multiple templates on the same page.

```javascript

`nested route
Add anonymous function in the parent route which will make the newroutes a child route of the newroute parents route. This will automaticaaly inherit the newroute as parent and you do not need to 
specify the path: /newroute/:newroute_id `

Router.map(function() {
  this.route('orders');
  this.route('newroute', function(){
      this.route('newroutes', {path: '/:newroute_id'});
  });
});

``` 

To make the nesting work correctly, a few more changes are needed. From here we go to the `#each` in our newroute template and change it to:

```html

`because newroutes is now a child of newroute we need to change the link to route name to be newroute.newroutes`
{{#each model as |name|}}
    {{#link-to "newroute.newroutes" name}}
        id - {{name.id}}
    {{/link-to}}
        ::: name - {{name.name}}<br>    
{{/each}}

{{outlet}}

```

Outcome

![image](https://cloud.githubusercontent.com/assets/17876815/13922421/78c7ace2-ef7c-11e5-9356-c3298802c623.png)


###Models and Services
To centralise the data we will create services. Services are long living objects available throughout the app (singleton). For this the data will be stored in a data repository service.

`ember gemerate service <service-name>`

![image](https://cloud.githubusercontent.com/assets/17876815/13931309/03eb0172-efa3-11e5-82fb-8e524b6356d5.png)

Services are made available within other objects by using `Ember.inject.service()`

```javascript

`app\service\store.js`
export default Ember.Service.extend({
    getOrders(){
    `The data was moved over from the newroute model hook into the getOrders function`
       return [{ id:'1', name:'myname' },
        { id:'2', name:'myname2' }
        ];
    }
});

`app\routes\newroute.js`
export default Ember.Route.extend({
    model() {
    	const store = this.get('store');
        return store.getOrders();
     },
    
    `store: equals local name of service and ('store') equals the name of the service to inject`
    store: Ember.inject.service('store')
    `because the service name matches the property name you can leave or drop the service name and ember will infer it from property 	     name e.g. store: Ember.inject.service()`
});

```

After injection, the store service becomes available as the "store" property.

To finalise the code and make sure the data is used correctly everywhere, where it is needed a few more changes are needed. Final code snippet below:

```javascript

`app/services/store`
export default Ember.Service.extend({
    getOrdersById(id){
        const orders = this.getOrders();
        return orders.findBy('id',id); `the find by was moved from newroutes.js`
    },
    
    getOrders(){
       return [{ id:'1', name:'myname' },
        { id:'2', name:'myname2' }
        ];
    }
});

`app/routes/newroute/newroutes.js`
export default Ember.Route.extend({
    model(params) {
        const id = params.newroute_id;
        const store = this.get('store');
        return store.getOrdersById(id);
    },
    
    store: Ember.inject.service()
});

```
