
computed
====

computed可以做计算的操作
----
1，computed实际上是一个我们定义class时候的get方法---通过Objec.definedProperty
给他设置get和set方法----表面上你是通过名字调用，实际上你是调用的这个方法。

在vue中会有更加复杂的处理，比如computed其实是有缓存的，可以节约性能。
在dom重新渲染时，computed内部的量没有改变，则会使用缓存，节省性能。

测试代码        

    import Vue from 'vue'
    
    new Vue({
      el: '#root',
      template: `
          <div>
            <p>Name : {{name}}</span>
            <p>Name : {{getName()}}</span>
            <p>Number : {{number}}</p>
            <p><input type="text" v-model="number"></p>
            <p>firstName : <input type="text" v-model="firstName"></p>
            <p>lastName : <input type="text" v-model="lastName"></p>
           </div>
      `,
      data: {
        firstName: 'Jocky',
        lastName: 'Lou',
        number: 0
      },
      computed: {
        name () {
          console.log(`computed name`, this.firstName)
          return `${this.firstName} ${this.lastName}`
        }
      },
      methods: {
        getName () {
          console.log(`methods name`, this.firstName)
          return `${this.firstName} ${this.lastName}`
        }
      }
    })
    
![](/images/blog/computed.png)

初始时，computed和getName都进行了计算，      
改变input框内的数字，computed没有计算。      
而只有当改变firstName或者lastName的时候，computed才会计算一次，并且再次将新值缓存起来。        
此后继续改变number的值，computed又会继续使用缓存。
而getName方法不论在什么时候都进行了计算，性能对比一目了然。


computed还可以做设置的操作
----
代码如下        
在template中      

       template: `
           <div>
             <p>Name : {{name}}</span>
             <p>Name : {{getName()}}</span>
             <p>Number : {{number}}</p>
             <p><input type="text" v-model="number"></p>
             <p>firstName : <input type="text" v-model="firstName"></p>
             <p>lastName : <input type="text" v-model="lastName"></p>
             <p>name : <input type="text" v-model="name"></p>
            </div>
       `,

在computed中

    computed: {
        name: {
          get () {
            console.log(`computed name`, this.firstName)
            return `${this.firstName} ${this.lastName}`
          },
          set (name) {
            const names = name.split(' ')
            this.firstName = names[0]
            this.lastName = names[1]
          }
        }
      },
      
      
改变input[v-model='name'']的值，其他地方也会改变。        
但是**不推荐**用computed做设置的操作，因为computed是通过多重数据组合而来的一个新的数据，
组合容易，但是将每个数据拆开来设置则会困难很多，极容易犯错。导致各种重复计算和循环，逻辑不清。     
所以，不到万不得已，不建议使用computed中的set功能。


watch
====
watch监听数据，页面刚开始渲染时，是不会执行的。      
想要在一开始就执行，需要加上`immediate：true`

      obj: {
          handler () {
            console.log('obj is changed')
          },
          imemediate: true,
      }
      
 对于对象`obj:{a:1}`直接给a赋值`obj.a=44`，watch监听obj是监听不到的,测试可以使用深度监听`deep:true`。        
 
        obj: {
              handler () {
                console.log('obj is changed')
              },
              imemediate: true,
              deep: true
         }
         
 `deep:true`会把对象的每一层内部的变量都监听到，如果对象结构复杂，性能开销自然就会比较大。     
 
 我们可以优化一下，使用字符串`obj.a`只监听obj的a属性。
 
     'obj.a': {
          handler () {
            console.log('obj is changed')
          },
          imemediate: true
      }
 
 
这里建议： 平时使用时，尽量不要在watch和computed里面去修改依赖的值，极容易出现无限循环的情况（当然控制台会在此前直接报错）。