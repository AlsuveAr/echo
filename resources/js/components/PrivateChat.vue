<template>
    <div class="container">
        <hr>
        <div class="row">
        	<div class="col-sm-12">
        		<textarea rows="10" class="form-control" readonly>{{ messages.join('\n') }}</textarea>
        		<hr>
        		<input type="text" class="form-control" v-model="textMessage" @keyup.enter="sendMessage">
        	</div>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['room'],
    	data() {
    		return {
    			messages: [],
    			textMessage: ''
    		}
    	},
        mounted() {
            //console.log(this.room)
            window.Echo.private('room.' + this.room.id)
        			.listen('PrivateChat', ({data}) => {
        				this.messages.push(data.body)
        			});
        },
        methods: {
        	sendMessage() {
        		axios.post('/messages', {body: this.textMessage, room_id: this.room.id });

        		this.messages.push(this.textMessage);

        		this.textMessage = '';
        	}
        }
    }
</script>
