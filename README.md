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





 

  
