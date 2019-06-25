

# Schematic: Agora’s iOS SDK Mapping Guide for TokBox Video functionalities





Importing SDK

import OpenTok
import AgoraRtcEngineKit
Accessing SDK / Initialization

TokBox: session, publisher, subscriber

    lazy var session: OTSession = {
        return OTSession(apiKey: kApiKey, sessionId: kSessionId, delegate: self)!
    }()
    
    lazy var publisher: OTPublisher = {
        let settings = OTPublisherSettings()
        settings.name = UIDevice.current.name
        return OTPublisher(delegate: self, settings: settings)!
    }()
    
    var subscriber: OTSubscriber?

Agora: a single instance

let agoraKit: AgoraRtcEngine 

Toggle Camera
TokBox: publisher.cameraPosition
    @objc func switchCamera(_ sender: UIButton) {
        
        
        if(publisher.cameraPosition == .back)
            
        
            publisher.cameraPosition = .front
            
        }else{
            
            publisher.cameraPosition = .back
        
        
    

Agora: agoraKit.switchCamera()
@IBAction func didClickSwitchCameraButton(_ sender: UIButton) {
    sender.isSelected = !sender.isSelected
    agoraKit.switchCamera()
    resetHideButtonsTimer()
}


Leave Room / Channel
TokBox: sessionDidDisconnect(_ )
    func sessionDidDisconnect(_ session: OTSession) {
        print("Session disconnected")
    
    

Agora: agoraKit.leaveChannel(nil)
func leaveChannel() {
    agoraKit.leaveChannel(nil)
    logMessage(messageText: "Attempting to leave channel")
    agoraKit = nil
}

Join Room / Channel
TokBox: sessionDidConnect()
func sessionDidConnect(_ session: OTSession) {
        print("Session connected")
        doPublish()
    
    

Agora: agoraKit.joinChannel()

func joinChannel() {
    agoraKit.joinChannel(byKey: nil, channelName: "demoChannel1", info:nil, uid:0) {[weak self] (sid, uid, elapsed) -> Void in
        if let weakSelf = self {
            weakSelf.agoraKit.setEnableSpeakerphone(true)
            UIApplication.shared.isIdleTimerDisabled = true
       
    
}


Configure Video
Twilio: TVIConnectOptions.init(token: accessToken) {(builder) in}
Agora: agoraKit.setVideoEncoderConfiguration()
Preview Video
Twilio: self.prepareLocalMedia()
Agora: agoraKit.enableVideo()  // Enables video mode.


Resolution and frame rate for a video
TokBox: OTPublisherSettings *_publisherSettings = [[OTPublisherSettings alloc] init];

OTPublisherSettings *_publisherSettings = [[OTPublisherSettings alloc] init];
_publisherSettings.cameraResolution = OTCameraCaptureResolutionHigh;
_publisherSettings.cameraFrameRate = OTCameraCaptureFrameRate30FPS;
_publisher = [[OTPublisher alloc]
              initWithDelegate:self
              settings:_publisherSettings];

Agora: AgoraVideoEncoderConfiguration()

let config = AgoraVideoEncoderConfiguration(size: size, frameRate: frameRate, bitrate: bitrate, orientationMode: orientationMode, degradationPreference: degradationPreference)

agoraKit.setVideoEncoderConfiguration(config)



