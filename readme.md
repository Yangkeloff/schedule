## 坑
1. 异步父子组件传值需要注意
在异步函数中更新数据的时候,必须手动调用`this.$apply()`,才会触发脏数据检查流程的运行
2. 普通自定义方法在methods对象外声明，与methods平级
![img](http://ww2.sinaimg.cn/large/006tNc79ly1g430qlr1q4j30ln08mdhv.jpg)
WePY中的methods属性，因为与Vue中的使用习惯不一致，非常容易造成误解，这里需要特别强调一下：WePY中的methods属性只能声明页面wxml标签的bind、catch事件，不能声明自定义方法，这与Vue中的用法是不一致的。
  ```
  // 正确示例
  import wepy from 'wepy';
  export default class MyComponent extends wepy.component {
      methods = {
          bindtap () {
              let rst = this.commonFunc();
              // doSomething
          },

          bindinput () {
              let rst = this.commonFunc();
              // doSomething
          },
      }

      //正确：普通自定义方法在methods对象外声明，与methods平级
      customFunction () {
          return 'sth.';
      }
  }
  ```
3. 在wepy中使用VantUI组件
使用npm或clone到本地,将vant的dist文件夹复制到项目components下并重命名(如:vant),在page中使用:
    ```
    //script
    export default class xxx extends wepy.page {
        config = {
            ...
            "usingComponents": {
            'van-button': '../components/vant/button/index',
            }
            ...
        }
        ...
    }

    //template
    <van-button type="primary">按钮</van-button>
    ```
4. open-data标签的头像圆角
常规方法只设置`border-radius: 50%;`无效,需要同时设置
```
border-radius: 50%;
overflow: hidden;
```
5. wepy使用云函数
- project.config.json中添加`"cloudfunctionRoot": "./functions/"`并在项目**根目录**创建文件夹functions
- 在微信开发者工具中右键`functions | mini-program`文件夹,新建Node.js云函数
- 在编辑器中对index.js进行编写,完成后在微信开发者工具中右键此云函数上传并部署
- 调用:
    ```
    wx.cloud.callFunction({
    // 云函数名称
    name: 'add',
    // 传给云函数的参数
    data: {
        a: 1,
        b: 2,
    },
    })
    .then(res => {
    console.log(res.result) // 3
    })
    .catch(console.error)
    ```
6. 什么时候使用`setStorage`什么时候使用`setStorageSync`?
前者为异步操作，后者为同步操作，若后续的操作依赖于更改storage后的数据，则需要同步，否则后续操作执行时还是使用的未更新的数据。若后续操作无需用到更改的storage数据，则不需要立即同步，这时用异步操作即可，节省内存