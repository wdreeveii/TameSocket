#TameSocket
##WebSockets Tamed!!

###TameSocket is a jquery library that will setup a new WebSocket with the following handy features:
1. Detect if the client computer has gone to sleep by setting a periodic interval that will reset the connection when it detects that the client has not been running for (default) 60 seconds.
2. Reconnect the WebSocket if it is closed for any reason. Possible causes of closed connections are network problems or computer sleep mode. There is a configurable reconnect delay with a default of 5 seconds.
3. Buffer and rate limit incoming messages to a given minimum message processing interval (default) 1 second.
4. Register an event handler to reconnect the websocket when the browser 'online' event is emitted as a browser exits "Work Offline" mode.

All you have to do is include TameSocket.js before your websocket code and then create a new TameSocket.

###Example:
```
function process_messages(bufferedMsgs) {
	while (bufferedMsgs.length > 0) {
		var event_msg = JSON.parse(bufferedMsgs.shift());
		if (event_msg["Type"] == "edit_") {
			var event_data = event_msg["Data"];
      // do something with event_data;
		} 
	}
}
var socket = TameSocket({
	target: 'ws://' + window.location.host + '/engine/ws',
	msgProcessor: function(bufferedEvents) {
		requestAnimationFrame(function() {
			process_messages(bufferedEvents);
			draw_display();
		});
	}
});
```
The result returned by TameSocket is a normal WebSocket object so you can interract with it the same way you interract with all WebSockets.
###Available configuration options and defaults:
| Option | Default | Description |
|:-------|:-------:|:------------|
| target | null | The address of the WebSocket you want to connect to. Required. |
| sleepTimeout | 60000 | The number of milliseconds a TameSocket should use to detect a sleep. |
| reconnectDelay | 5000 | The number of milliseconds a TameSocket will wait before reconnecting to the WebSocket. |
| msgMinInterval | 1000 | The number of milliseconds a TameSocket will buffer messages between calls to msgProcessor. |
| msgProcessor | null | The message processing function. This function will take an array with each element representing the raw data received from the WebSocket. This function is responsible for clearing events once they have been processed. The recommended way for cleaning up this array is to iterate through it and shift results out of the array. See the example for a demonstration. |
