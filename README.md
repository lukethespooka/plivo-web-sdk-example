Forked 21 Apr 2017

Plivo Web SDK Examples
======================

We'll be adding all our Web SDK examples and demos here. 

Web-Phone Example
-----------------

This is a very simple demo showing how you can make phone calls from the web browser to both sip addresses and PSTN phone numbers
There is a [live web phone demo](http://s3.amazonaws.com/plivowebrtc/phone.html) The entire source code is available in [phone/phone.html](https://github.com/cachrisman/plivo-web-sdk-example/blob/patch-1/phone/phone.html). To use the live demo, please sign up for a Plivo account here: https://manage.plivo.com/accounts/register/ then make a Plivo Endpoint here: https://manage.plivo.com/endpoint/create/. You must use a Plivo Endpoint to log into the WebSDK demo page. Do not try to use your Plivo account credentials to log into the demo.

### Where it all begins
In the document's ready function is where the Plivo JS Objects is initialised

    $(document).ready(function() {
            Plivo.onWebrtcNotSupported = webrtcNotSupportedAlert;
            Plivo.onReady = onReady;
            Plivo.onLogin = onLogin;
            Plivo.onLoginFailed = onLoginFailed;
            Plivo.onLogout = onLogout;
            Plivo.onCalling = onCalling;
            Plivo.onCallRemoteRinging = onCallRemoteRinging;
            Plivo.onCallAnswered = onCallAnswered;
            Plivo.onCallTerminated = onCallTerminated;
            Plivo.onCallFailed = onCallFailed;
            Plivo.onMediaPermission = onMediaPermission;
            Plivo.onIncomingCall = onIncomingCall;
            Plivo.onIncomingCallCanceled = onIncomingCallCanceled;
            
            Plivo.init();

        });

### Registering
The UI starts off with a basic login screen for accepting your Plivo Endpoint Credentials. To use the entered values to log in, use the following code

    function login() {
            Plivo.conn.login($("#username").val(), $("#password").val());
    }

The onLogin listener function, which was registered at the start is then invoked when Plivo sends back the onLogin event

    function onLogin() {
            $('#status_txt').text('Logged in');
            $('#login_box').hide();
            $('#logout_box').show();
            $('#callcontainer').show();
    }

### Making a call
Enter the number or sip uri in the post login UI and click the call button. This action causes the following code to run

    if ($('#make_call').text() == "Call") {
            var dest = $("#to").val();
            if (isNotEmpty(dest)) {
              $('#status_txt').text('Calling..');
              Plivo.conn.call(dest);
              $('#make_call').text('End');
            }
            else{
              $('#status_txt').text('Invalid Destination');
    }    
The code after discarding validations and UI state changes boils down to one line.

    Plivo.conn.call(dest);

### Handling Incoming calls
By creating the onIncomingCall listener, the Plivo JS object can handle calls coming into the Plivo Endpoint. 

    function onIncomingCall(account_name, extraHeaders) {
            console.log("onIncomingCall:"+account_name);
            console.log("extraHeaders=");
            for (var key in extraHeaders) {
                console.log("key="+key+".val="+extraHeaders[key]);
            }
            IncomingCallUI();
      }

The UI which shows an incoming call is rendered with the above code. The two actions that can be performed now are answer or reject. The code for answering looks like this.

    function answer() {
            console.log("answering")
            $('#status_txt').text('Answering....');
            Plivo.conn.answer();
            callAnsweredUI()
    }

The reject code looks like this

    function reject() {
            callUI()
            Plivo.conn.reject();
    }

### Terminating a call
This code may be used to terminate a call. 

    else if($('#make_call').text() == "End") {
            $('#status_txt').text('Ending..');
            Plivo.conn.hangup();
            $('#make_call').text('Call');
            $('#status_txt').text('Ready');
    }

Web-Conference Example
----------------------

The source code is availble in conference/conference.html. To test it out, use your plivo endpoint username and password in the login() function. 
