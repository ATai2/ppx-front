<template>
  <el-form ref="form" :model="form" label-width="80px">
    <el-form-item label="连接地址：">
      <el-input v-model="form.url" label-width="80px"></el-input>
      <el-button @click="confirmUrl">确认</el-button>
    </el-form-item>

    <el-form-item label="活动形式">
      <el-input type="textarea" v-model="form.desc" class="textArea"></el-input>
    </el-form-item>
  </el-form>
</template>

<script>
import SockJS from "sockjs-client";
import Stomp from "stompjs";
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data() {
    return {
      stompClient: "",
      timer: "",
      ws: null,
      form: {
        url: "http://localhost:9999/ppxws",
        text: ""
      },
      msgList: []
    };
  },
  mounted() {
    this.initWebSocket();
  },
  beforeDestroy: function() {
    // 页面离开时断开连接,清除定时器
    this.disconnect();
    clearInterval(this.timer);
  },
  methods: {
    confirmUrl() {
      initWebSocket();
    },
    initWebSocket() {
      this.connection();
      let that = this;
      // 断开重连机制,尝试发送消息,捕获异常发生时重连
      this.timer = setInterval(() => {
        try {
          that.stompClient.send("test");
        } catch (err) {
          console.log("断线了: " + err);
          that.connection();
        }
      }, 5000);
    },
    connection() {
      // 建立连接对象
      let socket = new SockJS(this.form.url);
      // 获取STOMP子协议的客户端对象
      this.stompClient = Stomp.over(socket);
      // 定义客户端的认证信息,按需求配置
      let headers = {
        Authorization: ""
      };
      // 向服务器发起websocket连接
      this.stompClient.connect(
        headers,
        () => {
          this.stompClient.subscribe(
            "/topic/public",
            msg => {
              // 订阅服务端提供的某个topic
              console.log("广播成功");
              console.log(msg); // msg.body存放的是服务端发送给我们的信息
            },
            headers
          );
          this.stompClient.send(
            "/app/chat.addUser",
            headers,
            JSON.stringify({ sender: "", chatType: "JOIN" })
          ); //用户加入接口
        },
        err => {
          // 连接发生错误时的处理函数
          console.log("失败");
          console.log(err);
        }
      );
    }, //连接 后台
    disconnect() {
      if (this.stompClient) {
        this.stompClient.disconnect();
      }
    } // 断开连接

    // initWs() {
    //   const wsUri = this.form.url;
    //   this.ws = new WebSocket(wsUri);
    //   this.ws.onopen = this.websocketonopen;
    //   this.ws.onerror = this.websocketonerror;
    //   this.ws.onmessage = this.websocketonmessage;
    //   this.ws.onclose = this.websocketclose;
    // },
    // websocketonopen() {
    //   console.log("WebSocket连接成功");
    //   this.form.text += "WebSocket连接成功";
    // },
    // websocketonerror(e) {
    //   //错误
    //   console.log("WebSocket连接发生错误");
    //   this.form.text += "WebSocket连接发生错误";
    // },
    // websocketonmessage(e) {
    //   //数据接收
    //   const redata = JSON.parse(e.data); //注意：长连接我们是后台直接1秒推送一条数据，
    //   //但是点击某个列表时，会发送给后台一个标识，后台根据此标识返回相对应的数据，
    //   //这个时候数据就只能从一个出口出，所以让后台加了一个键，例如键为1时，是每隔1秒推送的数据，为2时是发送标识后再推送的数据，以作区分
    //   console.log(redata.value);
    // },
    // websocketsend(agentData) {
    //   //数据发送
    //   this.websock.send(agentData);
    // },
    // websocketclose(e) {
    //   //关闭
    //   console.log("connection closed (" + e.code + ")");
    // },
    // confirmUrl() {
    //   initWs();
    // }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.textArea {
  height: 30%;
}
</style>
