##Ember Training and Learnings

###Important links:
* http://campus.codeschool.com/courses/try-ember/level/1/section/1/video/1
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

`Router.map(function() {
    ---Application endpoints mapped here---
});`

`Router.map(function() {
    this.route('orders', {path: '/orders'});
});`

___OR___

`Router.map(function() {
    this.route('orders');
});`

(path will be inferred)

![image](https://cloud.githubusercontent.com/assets/17876815/13815628/a570b224-eb8b-11e5-8cf7-23d70b6b9fc8.png)

(based on template created orders.hbs)


  
