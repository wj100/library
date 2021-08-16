// socket通信组件
<script>
export default {
    name: 'app-socket',
    props: {
        url: {
            type: String,
            default: 'ws://echo.websocket.org'
        },
        autoReconnect: {
            type: Boolean,
            default: false
        },
        autoReconnectDelay: {
            type: Number,
            default: 5000
        }
    },
    data(){
        return {

        }
    },
    methods: {
        send(msg){
            if(this.socket){
                this.socket.send(JSON.stringify(msg))
            }
        },
        close(){
            if(this.socket){
                this.socket.close()
            }
        },
        __creatConnection(){
            if(this.socket) this.socket = null
            this.socket = new WebSocket(this.url)
            this.socket.onopen = () => {
                this.__onopen()
            }
            this.socket.onmessage = (evt) => {
                this.__onmessage(evt)
            }
            this.socket.onerror = (err) => {
                this.__onerror(err)
            }
            this.socket.onclose = () => {
                this.__onclose()
            }
        },
        __onopen(){
            this.$emit('onOpen')
        },
        __onmessage(evt){
            this.$emit('onMessage', JSON.parse(evt.data))
        },
        __onerror(err){
            this.$emit('onError', err)
            if(this.autoReconnect){
                this.$emit('onReconnect')
                console.log('ws将在'+this.autoReconnectDelay/1000+'秒后自动重连')
                setTimeout(() => {
                    this.__creatConnection()
                }, this.autoReconnectDelay)
            }
        },
        __onclose(){
            this.__destroy()
            this.$emit('onClose')
        },
        __destroy(){
            this.socket = null
        }
    },
    render(){
        return {}
    },
    mounted(){
        this.__creatConnection()
    },
    beforeDestroy(){
        this.__destroy()
    }
}
</script>