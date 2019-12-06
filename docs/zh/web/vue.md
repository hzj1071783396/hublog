# 组键
## 创建方式
    第一种方式
    	var com1 = Vue.extend({
			template:'<h3>这是使用Vue.extend创建的组件</h3>'
		})
		Vue.component('mycom1',com1)
		或
		Vue.component('mycom1',Vue.extend({
			template:'<h3>这是使用Vue.extend创建的组件</h3>'
		}))
		
	第二种方式
	    Vue.component('mycom1',{
			template:'<h3>这是使用Vue.extend创建的组件111</h3>'
		})
		
	第三种方式（优点，有代码提示）
	    在body里边，app的外边
	    <template id="temp3">
			<div>
				<h3>这是使用Vue.extend创建的组件</h3>
			</div>
		</template>
		
	    Vue.component('mycom3',{
	        template:'#temp3'
	    })
	
## 私有组键
    data必须是一个方法，把值返回（返回的好处是，每用一个data时都是一个新的对象，互不影响）
	    components:{
			login:{
				template:'<h1>私有组键--{{msg}}</h1>'
			},
			data:function(){
				return {
					msg:'组键中的data'
				}
			}
		}

## 组键的自定义属性 props
    	 components:{
			login:{
				template:'<h1>私有组键--{{msg}}</h1>',
				props:['parentmsg']  //相当于父组件中的data
				
			},
			data:function(){
				return {
					msg:'组键中的data'
				}
			}
		}
		
## 父子组键
    传值 使用时用v-bind (:)
        子组件不能直接使用父组件的值，但是在使用的时候可以用数据绑定的方法把父组件传入的子组件中
        
    传方法 用v-on (@)
    
    使用传递过来的方法
        components:{
			login:{
				template:'<h1 @func='myclick'>私有组键--{{msg}}</h1>',
				props:['parentmsg']  //相当于父组件中的data
				
			},
			data:function(){
				return {
					msg:'组键中的data'
				}
			},
			methods:{
			    myclick:function(){
			        this.$emit('func','param1',param2...) //后边几个是传递过来的参数
			    }
			}
		}
    
## 父子组键实现v-model效果
    v-model是vue自带的，也可以自定义，但是有可能造成冲突。所以我们使用另一种方法
    
    子组件中
``` html
<template>
    <div class="my-input-main">
        <div class="component-title"><span>{{title}}</span></div>
        <div class="my-input-input"><input :type="type" v-model="thisValue"/></div>
    </div>
</template>

<script>
    export default {
        name: "myInput",
        props:['type','length','title','myValue'],
        data(){
            return {
                thisValue:this.myValue
            }
        },
        watch:{
            thisValue(){
                this.$emit('update:myValue',this.thisValue)
            }
        }

    }
</script>
```
    父组件中
```
<template>
    <div class="content-main">
        <my-input type="text" :my-value.sync="text" title="单行文本"/>
    </div>
</template>

<script>
    export default {
        name: "index",
        data(){
            return{
                text:"我是自定义组件"
            }
        }
    }
</script>
```
    
    
        
    
