<script>

const ws = new WebSocket("ws://localhost:8080/ws");
ws.onopen = function(event) {
    console.log("WebSocket connection established.");
};
ws.onmessage = function(event) {
	const messagesJson = JSON.parse(event.data)
	messages = messagesJson
};
ws.onerror = function(error) {
    console.error("WebSocket error:", error);
};
ws.onclose = function(event) {
    console.log("WebSocket connection closed.");
};

const handleSubmit = (e) => {
	e.preventDefault();
	const payload = {name, message};
	ws.send(JSON.stringify(payload))
	message = '';
}

let message
let name
let messages = []
let messagesFormatted = []

$: {
	messagesFormatted = messages.map(msg => (`${msg.name}: ${msg.message}`))
	messagesFormatted = messagesFormatted.join('\n')
  }

</script>

<main>
	<div>
		<div><input type="text" bind:value={name} placeholder="name"></div>
		<div><textarea class="big" value={messagesFormatted} readonly/></div>
		<form on:submit={handleSubmit}>
			<div><textarea bind:value={message} placeholder="say something..."/></div>
			<div><button tyoe="submit">Click dis</button></div>
		</form>
	</div>
</main>

<style>
.big{
	height: 60vh;
}

</style>