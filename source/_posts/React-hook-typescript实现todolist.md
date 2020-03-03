---
title: React-hook typescript实现todolist
date: 2020-03-03 11:12:22
tags: React,TypeScript
---

## React-hook typescript实现todolist




### 需求
	1.列表显示
	2.输入框添加
	3.删除对应项
	4.显示/改变完成状态

### 思路

使用useReducer()进行添加/删除/修改状态操作，每条记录state包含content（内容）和state（完成状态）

1. 定义一个myReducer
```
const myReducer=(state:any,action:any)=>{
    switch(action.type){
        case 'ADD':
            return {
                ...state
            }
            break;
        case 'DEL':
            return {
                ...state
            }
            break;
        case 'CHANGE_STATE':
            return {
                ...state
            }
            break;
        default:
            return state
    }
}
```
2. 添加输入部分和显示部分,并给予初始化数据;使用useReducer()实现操作
```
export default function App(){

    const [state,dispatch] = useReducer(myReducer,{
        list: [
            {
              content:'吃饭',
              state:false
            },
            {
              content: '买菜',
              state: false
            },
            {
              content: '做饭',
              state: false
            }
          ]
    })//赋予初始值list数组
    
    const List = (props:any)=>{
        const {list}=props
        console.log(props)
        return (
            <ul>
                {
                    list.map((item:{content:React.ReactNode,state:Boolean},index:string)=>{
                        return (
                        <li key={index+'a'}>
                        	<span style={{marginLeft:'10px',marginRight:'10px'}}>{item.state?'已完成':'未完成'}</span>
                            <span>{item.content}</span>
                        </li>
                        )
                    })
                }
            </ul>{/*注意参数的数据类型TypeScript是静态强类型语言*/}
        )
    }

    return (
        <div>
            <p>todolist</p>
            <div>
                <input type="text"/><button>添加</button>
            </div>
            <List list={state.list}/>{/**将父组件的值传给子组件 */}
        </div>
    )
}
```
至此能够静态地将list列表内的数据展示出来

3. 添加项目
为了添加内容到列表,需要获取`<input/>`中的值,此时可以选用useRef操作.
`<input ref={inputEl} type="text"/>`,父元素中`const inputEl = useRef<HTMLInputElement>(null)//元素渲染之前都是null,所以初始化设置为null,注意初始化`
给按钮绑定事件并输出目标值
```
function handleAdd() :void{//注意返回值
        if(inputEl.current){
            console.log(inputEl.current.value)
        }
}	
    
return (
	...
	<button onClick={()=>handleAdd()}>添加</button>
	...
)
```
能够获取到input的当前值之后就需要将他添加到list当中,继续在function handleAdd()中添加dispatch操作
```
function handleAdd() :void{
        if(inputEl.current){
            console.log(inputEl.current.value)
            dispatch({type:'ADD',val:inputEl.current.value})//action是一个对象
        }
    }
```
修改reducer
```
case 'ADD':
return {
	...state,
	list: state.list.concat([{		
		content:action.val,
		state:false
	}])//不能修改原始的state
}
break;
```
至此ADD功能已经添加完毕

4. 删除项目
这个功能关键在于获取项目的下标,从而删除对应的项
对列表中的元素绑定点击操作,并将此事件传递给父组件
```
...
const {list,handleDelProps}=props
return (
	<li key={index+'a'}>
		<span style={{marginLeft:'10px',marginRight:'10px'}{item.state?'已完成':'未完成'}</span>
		<span onClick={()=>handleDelProps(index)}>{item.content}</span>
		{/*需要传递下标*/}
</li>
)
...
...

return (
	...
	<List list={state.list} handleDelProps={handleDel}/>{/*将父组件的值传给子组件*/}
	...
)
//编辑handleDel方法向reducer dispatch一个action
function handleDel(index: string) :void{
	console.log(index)
	dispatch({type:'DEL',target:index})
}
...
...
...
case 'DEL':
	return {
		...state,
		state:state.list.splice(action.target,1)
	}
```
5. 改变状态
也按照删除的思路来写
```
case 'CHANGE_STATE':
	return {
    	...state,
        state:state.list[action.aim].state=!state.list[action.aim].state
    }
    break;
```
### 总结
总体来说,由于demo比较简单,typescript的项目与javascript的项目没有太大的差别,只是部分参数需要限制数据类型
hook API确实提供了另一个实现业务逻辑的方向,因为不需要对组件实例化,所以函数组件配合hooks在同样的业务逻辑下的性能开销可能更小一些;脱离了生命周期函数,一定程度上也提高了代码可读性.





### 完整代码

```
import React, { useRef,useReducer } from 'react';

const myReducer=(state:any,action:any)=>{
    switch(action.type){
        case 'ADD':
            return {
                ...state,
                list: state.list.concat([{
                    content:action.val,
                    state:false
                }])//不能修改原始的state
            }
            break;
        case 'DEL':
            return {
                ...state,
                state:state.list.splice(action.target,1)
            }
            break;
        case 'CHANGE_STATE':
            return {
                ...state,
                state: state.list[action.aim].state=!state.list[action.aim].state
            }
            break;
        default:
            return state
    }
}

export default function App(){
    const inputEl = useRef<HTMLInputElement>(null)//元素渲染之前都是null,所以初始化设置为null,注意初始化

    const [state,dispatch] = useReducer(myReducer,{
        list: [
            {
              content:'吃饭',
              state:false
            },
            {
              content: '买菜',
              state: false
            },
            {
              content: '做饭',
              state: false
            }
          ]
    })//赋予初始值list数组
    function handleDel(index:string) :void{
        console.log(index)
        dispatch({type:'DEL',target:index})
    }
    

    const List = (props:any)=>{
        const {list,handleDelProps,changeStateProps}=props
        return (
            <ul>
                {
                    list.map((item:{content:React.ReactNode,state:Boolean},index:string)=>{
                        return (
                        <li key={index+'a'}>
                            <span style={{marginLeft:'10px',marginRight:'10px'}} onClick={()=>changeStateProps(index)}>{item.state?'已完成':'未完成'}</span>
                            <span onClick={()=>handleDelProps(index)}>{item.content}</span>
                        </li>
                        )
                    })
                }
            </ul>
        )
    }

    function handleAdd() :void{
        if(inputEl.current){
            console.log(inputEl.current.value)
            dispatch({type:'ADD',val:inputEl.current.value})//action是一个对象
        }
    }
    
    function changeState(index:string) :void{
        dispatch({type:'CHANGE_STATE',aim:index})
    }
    


    return (
        <div>
            <p>todolist</p>
            <div>
                <input ref={inputEl} type="text"/><button onClick={()=>handleAdd()}>添加</button>
            </div>
            <List list={state.list} handleDelProps={handleDel} changeStateProps={changeState}/>{/**将父组件的值传给子组件 */}
        </div>
    )
}
```