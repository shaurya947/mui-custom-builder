# mui-custom-builder
Custom build delivery framework for Material-UI

*Note: This README will change as the project evolves, and may lag behind sometimes. Follow my comments on [callemall/material-ui/#262](https://github.com/callemall/material-ui/issues/262) for progress updates.*

The idea is as follows: Add a form page to [material-ui.com](www.material-ui.com) that provides the capability to produce a custom build. This form allows the developer to choose an *available* version of MUI and the components that are to be included in the custom build (could also choose all components). The form then sends an HTTP request to a node app running on heroku, where a custom build is performed using [webpack](https://webpack.github.io/). The build files are then available to the developer to download.

#### How to run this?

```
git clone https://github.com/shaurya947/mui-custom-builder.git
cd mui-custom-builder

npm i
node app.js
```

Then, the magic happens when you navigate to localhost using a query string like so:

```
http://localhost:3000/?v=x.x.x&components=ALL/comp1,comp2,comp3...
```

Examples:

```
http://localhost:3000/?v=0.13.4&components=AppBar,AutoComplete,RaisedButton
http://localhost:3000/?v=0.13.3&components=ALL
```

If everything goes smoothly, you'll see a message in the browser asking you to check the build folder of the version you specified. So, navigate to `webpack-builder/x-x-x/build` and you will find `bundle.js` and `bundle.js.map` (here `x-x-x` is the version number with hyphens instead of periods).

#### Using the build in your HTML

```html
<!DOCTYPE html>
<html>
<head>
</head>
<body>
	<div>works</div>
	<div id="sample"></div>
	<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.23/browser.min.js"></script>
	<script src="bundle.js"></script>
	<script type="text/babel">
		var MyComp = React.createClass({
			render() {
				return (<div>
					<mui.AppBar/>
					<mui.RaisedButton label="hooray" secondary={true} />
					</div>);
			}
		});
		ReactDOM.render(<MyComp />, document.getElementById('sample'));
	</script>
	</body>
</html>
```
