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
