# Environment Identifier

Some applications are published to multiple environments, such as 'development', 'staging', 'UAT' or 'production'. 
As the applications commonly look alike, users can get confused as to which environment they have logged into. 
This module adds an element and label to application pages so users can quickly identify an environment. 

![Environment identifier Display Example](images/BarDisplayExample.png)

## Version
1.0 - initial

1.1 Fixed script bug - change 'text' to 'title'

1.2 Optimised script code

1.3 Moved environment bar attachment to container from body; converted px to rem; updated readme for 6.12+

# Global Script Setup
1. Create a Global Script called "EnvironmentIdentifier"
2. Add the input parameters below to the Global Script
   1. BarPosition
   2. Environments
3. Drag a *JavaScript* action into the script
4. Add the Javascript below unchanged into the JavaScript code property
```javascript
/* Stadium Script 1.3 https://github.com/stadium-software/environment-identifier */
let position = ~.Parameters.Input.BarPosition || "top";
let environments = ~.Parameters.Input.Environments;
if (!['top', 'right', 'bottom', 'left'].includes(position)) {
	position = 'top';
}
for (let i = 0; i < environments.length; i++) {
    if (window.location.href.indexOf(environments[i].url) > -1 && !document.body.querySelector(".environment-top-bar")) {
        let environmentBar = document.createElement('div');
        environmentBar.classList.add(environments[i].class, "environment-top-bar", "environment-top-bar-" + position);
        environmentBar.textContent = environments[i].title;
        document.querySelector(".container").appendChild(environmentBar);
        if (position == 'top') {
            let hd = document.querySelector('.header');
            hd.style.setProperty('top', 'calc(' + window.getComputedStyle(hd).getPropertyValue("top") + ' + ' + window.getComputedStyle(document.body).getPropertyValue("--environment-bar-size") + ')');
        }
    }
}
```

## Type Setup
1. Add a type called "Environment" to the types collection in the Stadium Application Explorer
2. Add the following properties to the type
   1. url (Any)
   2. class (Any)
   3. title (Any)

![Type Setup](images/EnvironmentType.png)

## Template.Load or Page.Load Setup
1. Drag a *List* action into the Load event handler of Templates or Pages where the environment element must be shown
2. Call the List "EnvironmentsList"
3. Assign the *Environment* type to the *List*
4. Define the environments
   1. *url*: the url of the environment or part thereof (e.g. localhost)
   2. *class*: a class that will be attached to the environment identifying element when the url input matches the url in the address bar
   3. *title*: the name of the environment. This will be shown inside the environment identifier

Environments List Example
```json
= [{
	"url": "stadium.software/staging",
	"class": "staging",
	"title": "Staging"
},{
	"url": "uat.stadium.software",
	"class": "uat",
	"title": "UAT"
},{
	"url": "localhost",
	"class": "development",
	"title": "Development"
}]
```
5. Drag the "EnvironmentIdentifier" global script into the Event Handler (below the *List*)
6. Enter parameters for the script
   1. BarPosition: the location where the bar will be shown. Supported are: top, left, right or bottom. The default is top
   2. Environments: Select the List containing the environments defined above from the dropdown

![Script Parameters Example](images/GlobalScriptInputs.png)

## Environments Custom Styling
Add CSS to the Stylesheet for each environment class you defined in the "EnvironmentsList". Here you can define what the environment identifier will look like. 

For example: 

```css
.staging {
	background-color: #924370;
	color: white;
	font-size: 12px;
	font-weight: bold;
}
.uat {
	background-color: #97F48A;
	color: #555;
	font-size: 12px;
	font-weight: bold;
}
.development {
	background-color: #333;
	color: #fff;
	font-size: 12px;
	font-weight: bold;
}
```

## CSS
The CSS below is required for the correct functioning of the module. Variables exposed in the [*environment-variables.css*](environment-variables.css) file can be [customised](#customising-css).

### Before v6.12
1. Create a folder called "CSS" inside of your Embedded Files in your application
2. Drag the two CSS files from this repo [*environment-variables.css*](environment-variables.css) and [*environment.css*](environment.css) into that folder
3. Paste the link tags below into the *head* property of your application
```html
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/environment.css">
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/environment-variables.css">
``` 

### v6.12+
1. Create a folder called "CSS" inside of your Embedded Files in your application
2. Drag the CSS files from this repo [*environment.css*](environment.css) into that folder
3. Paste the link tag below into the *head* property of your application
```html
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/environment.css">
``` 

### Customising CSS
1. Open the CSS file called [*environment-variables.css*](environment-variables.css) from this repo
2. Adjust the variables in the *:root* element as you see fit
3. Stadium 6.12+ users can comment out any variable they do **not** want to customise
4. Add the [*environment-variables.css*](environment-variables.css) to the "CSS" folder in the EmbeddedFiles (overwrite)
5. Paste the link tag below into the *head* property of your application (if you don't already have it there)
```html
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/environment-variables.css">
``` 
6. Add the file to the "CSS" inside of your Embedded Files in your application

**NOTE: Do not change any of the CSS in the 'environment.css' file**

## Upgrading Stadium Repos
Stadium Repos are not static. They change as additional features are added and bugs are fixed. Using the right method to work with Stadium Repos allows for upgrading them in a controlled manner. 

How to use and update application repos is described here: [Working with Stadium Repos](https://github.com/stadium-software/samples-upgrading)

## Known Issues
When attaching the environment indicator to the top of the page, it can obstruct other elements attached to the same position. In such cases consider attaching the environment indicator to the bottom of the page instead. 