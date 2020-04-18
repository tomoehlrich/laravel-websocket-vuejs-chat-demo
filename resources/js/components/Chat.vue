<template>
    <div>
        <div class="box">
            <p v-if="!messages.length">Start typing the first message</p>

            <div v-for="message in messages">
                <my-message
                    v-if="message.user == userId"
                    :message="message.text"
                ></my-message>

                <message
                    v-if="message.user != userId"
                    :message="message.text"
                    :user="message.user"
                ></message>
            </div>
        </div>

        <form @submit.prevent="submit">
            <div class="field has-addons has-addons-fullwidth">
                <div class="control is-expanded">
                    <input class="input" type="text" placeholder="Type a message" v-model="newMessage">
                </div>
                <div class="control">
                    <button type="submit" class="button is-danger" :disabled="!newMessage">
                        Send
                    </button>
                </div>
            </div>
        </form>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                userId: Math.random().toString(36).slice(-5),
                messages: [],
                newMessage: ''
            }
        },
        mounted () {
            Echo.channel('chat')
                .listen('NewChatMessage', (e) => {
                    if(e.user != this.userId) {
                        this.messages.push({
                            text: e.message,
                            user: e.user
                        });
                    }
                });
        },
        methods: {
            submit() {
                axios.post(`${process.env.MIX_WEBSOCKET_SERVER_BASE_URL}/api/message`, {
                    user: this.userId,
                    message: this.newMessage
                }).then((response) => {
                    this.messages.push({
                        text: this.newMessage,
                        user: this.userId
                    });

                    this.newMessage = '';
                }, (error) => {
                    console.log(error);
                });

            }
        }
    }
</script>
