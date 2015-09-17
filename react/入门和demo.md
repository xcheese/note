React.js V 0.13.3

为方便观察测试，直接给出HTML，引用test.js，如无特殊提示，所有reactjs的测试代码都在此文件中

# HTML 

	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="utf-8">
	<title>Examples</title>
	<script src="//cdn.bootcss.com/react/0.13.3/react.js"></script>
	<script src="//cdn.bootcss.com/react/0.13.3/JSXTransformer.js"></script>
	</head>
	<body>
		<div id="example"></div>
    	<script type="text/jsx" src="src/test.js"></script>
	</body>
	</html>
	
# React.render() 和 { }

```
var arr = [1,2,3]
React.render(
    <div>
    	{
    		arr.map(function(value){
    			return <div>{value}</div>
    		})
    	}
    </div>,
    document.getElementById('example')
);	
var arr = [
	<h1>h1</h1>,
	<h2>h2</h2>,
];
React.render(
	<div>{arr}</div>,
	document.getElementById('example')
);
````
# 创建组件 React.createClass

```
//组件类首字母大写
var Xhello = React.createClass({
	render :function(){
		return <h1>hello ,{this.props.name}</h1>
	}
});
React.render(
	<Xhello name='mql' />,
	document.getElementById('example')
);

```

# this.props.children

```
//this.props.children 长度要大于1，不然map报错
var List = React.createClass({
	render:function () {
		return(
			<ol>
				{
					this.props.children.map(function(child){
						return <li>{child}</li>
					})
				}
			</ol>
		)
	}
})
React.render(
	<List>
		<span>1111</span>
		<span>2222</span>
		<span>333</span>
		<p>asdnasdk<a>aaaaa</a></p>
	</List>,
	document.getElementById('example')
)
```

# 初始化默认属性，getDefaultProps

```
var Title = React.createClass({
	getDefaultProps:function(){
		return{
			title:'sadasdasdas'
		}
	},
	render:function(){
		return (
			<h1>{this.props.title}</h1>
		)
	}
```

# 初始化默认状态，getInitialState

```
var LikeButton = React.createClass({
	getInitialState:function(){
		return {liked:false}
	},
	handleClick:function(){
		this.setState({liked:!this.state.liked})
	},
	render:function(){
		var text = this.state.liked ? 'like' : 'no like';
		return (
			<p onClick={this.handleClick}>
				You {text} this
			</p>
		)

	}
})
```


# findDOMNode

```
var Component = React.createClass({
	handleClick:function(){
		var val = React.findDOMNode(this.refs.input1).value;
		console.log(val)
	},
	render:function(){
		return(
			<div>
				<input type="text" ref="input1" />
				<input type="button" value="btn++" onClick={this.handleClick} />
			</div>
		)
	}
})
```










 