# Webrtc
WebRTC Video and Audio Calling with TURN Server

**Overview**
This project demonstrates how to set up a WebRTC video and audio calling feature, including the configuration of a TURN server on AWS EC2 for improved connectivity.

**Features**
	•	Real-time video and audio calling
	•	TURN server support for stable connections

** Requirements**
	•	Node.js installed on your system.
	•	AWS account for creating and managing EC2 instances.
	•	Domain name for TURN server (optional, but recommended).
	•	Basic knowledge of WebRTC and server management.

**Clone the Repository**
git clone https://github.com/yourusername/webrtc-video-audio-calling.git
cd webrtc-video-audio-calling

**Install Dependencies**
npm install

**Setting Up the WebRTC Application**
1.	Frontend: The frontend code is responsible for capturing video and audio streams, establishing peer-to-peer connections, and handling UI interactions.
	•	Implement WebRTC APIs to capture user media (video/audio).
	•	Use RTCPeerConnection to create and manage connections between peers.
	•	Handle ICE candidates and signaling.
	2.	Backend: The backend handles signaling, managing users, and integrating the TURN server.
	•	Set up a Node.js server using Express.
	•	Implement socket.io for signaling between peers.
	•	Integrate the TURN server to relay media when direct peer-to-peer connection is not possible.

**Setting Up the TURN Server**
A TURN (Traversal Using Relay NAT) server helps relay media traffic when direct peer-to-peer connections cannot be established, typically due to NAT or firewall restrictions.

**Create an EC2 Instance on AWS**

Launch EC2 Instance:
•	Choose an Ubuntu Server as the base image.
•	Select an instance type (t2.micro is sufficient for testing).
•	Configure security groups to allow traffic on ports 3478 (UDP) and 5349 (TCP) for TURN.

SSH into the Instance:
ssh -i "your-key.pem" ubuntu@your-ec2-public-dns

**Install and Configure Coturn**
sudo apt-get update
sudo apt-get install coturn

**Configure Coturn:**
sudo nano /etc/turnserver.conf

**Add or modify the following settings:**
listening-port=3478
tls-listening-port=5349
fingerprint
use-auth-secret
static-auth-secret=<your-auth-secret>
realm=............................com
total-quota=100
bps-capacity=0
stale-nonce
cert=/path/to/your_certificate.pem
pkey=/path/to/your_private_key.pem

**Enable and Start Coturn:**
sudo systemctl enable coturn
sudo systemctl start coturn

**Configure WebRTC to Use the TURN Server**
In your WebRTC client configuration, add the TURN server details:

const iceServers = [
    {
        urls: [
            "turn:staging-...................com:3478?transport=udp",
            "turn:staging-...................com:3478?transport=tcp"
        ],
        username: "user",
        credential: "password"
    }
];

const config = {
    iceServers: iceServers,
    iceTransportPolicy: "relay" // Force the usage of TURN server
};

const peerConnection = new RTCPeerConnection(config);

**Testing**
npm start

•	Open the application in your browser.
•	Test the video and audio calling features.

**Troubleshooting**
	•	Ensure the TURN server is running and accessible over the network.
	•	Check firewall settings to make sure required ports are open.
	•	Monitor logs for any connection or ICE candidate errors.
