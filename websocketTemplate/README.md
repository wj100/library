# websocket-template
将原生websocket连接封装成vue的无视图组件

使用方法
```javascript
<template>
  <div>
      <!-- app-socket没有视图，放在页面最后即可 -->
      <app-socket
          ref="socket"
          url="ws://127.0.0.1"
          autoReconnect
          @onOpen="socketOnOpen"
          @onMessage="socketOnMessage"
          @onError="socketOnError"
          @onClose="socketOnClose"
      />
  </div>
</template>
<script>
export default {
  methods: {
      socketOnOpen(){
          console.log('ws连接成功')
      },
      socketOnMessage(evt){
          console.log('ws来消息了：', evt)
      },
      socketOnError(err){
          console.log('ws连接错误：', err)
      },
      socketOnClose(){
          console.log('ws连接关闭')
      },
      socketSend(msg){
          this.$refs.socket.send(msg)
      },
      socketClose(){
          this.$refs.socket.close()
      }
  },
  // 个别场景中(Index组件中就用到了)，子组件层级不确定，又想调用本组件中的socket来发送消息
  // 可通过依赖注入，将本组件中的方法注入到子组件，子组件通过inject即可调用该方法
  provide(){
      return {
          socketSend: this.socketSend
      }
  }
}
</script>
```