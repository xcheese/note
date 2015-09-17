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


# findDOMNode，this.refs

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
# componentWillMount，componentDidMount

```
var Hello = React.createClass({
	getInitialState:function(){
		return {
			opacity:1.0
		}
	},
	componentWillMount:function(){
		this.timer = setInterval(function(){
			var opacity = this.state.opacity;
			opacity -=0.05;
			if(opacity < 0.1){
		 		opacity = 1.0;
			};
			this.setState({
				opacity:opacity
			})
		}.bind(this),100)
		
		console.log(1)

	},
	componentDidMount:function(){
		console.log(2)
	},
	render:function(){

		return(
			<div style={{opacity: this.state.opacity}}>
				Hello {this.props.name}
				<p>{this.timer}</p>
			</div>
		)

	}
```

# mixins

```
var SetIntervalMixin = {
  componentWillMount: function() {
    this.intervals = [];
  },
  setInterval: function() {
    this.intervals.push(setInterval.apply(null, arguments));
  },
  componentWillUnmount: function() {
    this.intervals.map(clearInterval);
  }
};

var TickTock = React.createClass({
  mixins: [SetIntervalMixin], // 引用 mixin
  getInitialState: function() {
    return {seconds: 0};
  },
  componentDidMount: function() {
    this.setInterval(this.tick, 1000); // 调用 mixin 的方法
  },
  tick: function() {
    this.setState({seconds: this.state.seconds + 1});
  },
  render: function() {
    return (
      <p>
        React has been running for {this.state.seconds} seconds.
      </p>
    );
  }
});

React.render(
  <TickTock />,
  document.getElementById('example')
);

```

# 生命周期，实例化
* prop -> state -> willMount -> render -> didMount

```
var Test = React.createClass({
	getDefaultProps:function(){
		console.log(1)
		return {p:1}
	},
	getInitialState:function(){
		console.log(2)
		return {s:2}
	},
	componentWillMount:function(){
		console.log(3)
	},
	componentDidMount:function(){
		console.log(5)
	},
	render:function(){
		console.log(4)
		return (
			<p>test声明周期</p>
		)
	}
})

React.render(
  <Test />,
  document.getElementById('example')
);
```

# 生命周期，存在期

1. componentWillReceiveProps  //更新prop、state时机
2. -> shouldComponentUpdate	   //return false;可以跳过render，加快速度，如果你确认prop和state都没有变化的话
3. -> componentWillUpdate	   
4. -> render
5. -> componentDidUpdate

# 生命周期，销毁

* componentWillUnmount	






























 