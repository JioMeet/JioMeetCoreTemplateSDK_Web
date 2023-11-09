<p align="center" >
  <img src="https://jiomeetpro.jio.com/assets/img/website/website_logo_header_light.svg" title="logo" float=center  height="150">
</p>

# 👋 JioMeet Web Core Template

Meet the JioMeet Web Core template—a robust SDK that seamlessly integrates meetings into your Angular and React applications. With the simplicity of renderCompositeView, you can now enhance collaboration in your app. Join calls, utilize audio, video, screenShare, whiteboard, and more—all effortlessly packaged in one user-friendly solution. Elevate your app's collaboration game with the JioMeet Core Template! 🌐🚀

For our React enthusiasts, we've got exciting news! Introducing separate components for rendering preview, conference UI, and additional hooks—making your React experience even more versatile and tailored to your needs. It's time to redefine collaboration in your applications!

## Table of Contents

- [👋 Introduction](#-jioMeet-web-core-template-sdk)
- [🚀 How to Install SDK](#-how-to-install-sdk)
- [🏁 Setup](#-setup)
- [🌟 Provider](#-provider)
- [🎨 Component](#-component)
- [🪝 Hooks](#-hooks)

## 🚀 How to Install SDK

You can install the SDK using npm :

To install via [NPM](https://www.npmjs.com/):

```bash
npm install @jiomeet/core-template-web
```

## 🏁 Setup

#### Register on JioMeet Platform:

You need to first register on Jiomeet platform. [Click here to sign up](https://platform.jiomeet.com/login/signUp)

#### Get your application keys:

Create a new app. Please follow the steps provided in the [Documentation guide](https://dev.jiomeet.com/docs/quick-start/introduction) to create apps before you proceed.

#### Get your Jiomeet meeting id and pin

Use the [create meeting api](https://dev.jiomeet.com/docs/JioMeet%20Platform%20Server%20APIs/create-a-dynamic-meeting) to get your room id and password

## 🌟 Provider

### 🔓 JMTemplateProvider

You need to wrap your top most component with these provider after that you can access any hook from the SDK

##### Example:

```typescript
import { JMTemplateProvider } from '@jiomeet/core-template-web';

const Client = ({ children }) => {
  return <JMTemplateProvider>{children}</JMTemplateProvider>;
};
const root = createRoot(document.getElementById('container'));
root.render(<Client />);
```

## 🎨 Component

### JMConferenceView

This component will give ready made UI for conference , all the user joined will be displayed in grid with there name and audio state and have pagination support as well after 8 peer

##### Parameters:

- **conferenceConfig?** : <code><a href="#ijmconferenceconfigurations">IJMConferenceConfigurations</a></code>
  After successfully joining the call , you can pass this prop to JMConferenceView to customize the in call User Interface.

##### Example:

```typescript
import {
  JMConferenceView,
  useJoin,
  IJMConferenceConfigurations,
} from '@jiomeet/core-template-web';

function App() {
  const { isCallStarted } = useJoin();

  // Define configuration options
  const conferenceConfig: IJMConferenceConfigurations = {
    configurations: {
      hideCallControls: true, // Set to true to hide call control buttons
      disableAudioOnlyMode: false, // Set to true to disable audio-only mode
      defaultMediaSettings: {
        // Specify default media settings
        trackSettings: {
          audioMuted: false, // Set to true to mute audio by default
          videoMuted: false, // Set to true to mute video by default
          audioInputDeviceId: '', // Specify audio input device ID if needed
          audioOutputDeviceId: '', // Specify audio output device ID if needed
          videoDeviceId: '', // Specify video device ID if needed
        },
        virtualBackgroundSettings: {
          isVirtualBackground: false, // Set to true to enable virtual background
          sourceType: 'none', // Specify the virtual background source type
          sourceValue: '', // Specify the source value for the virtual background
        },
      },
      disableToasts: false, // Set to true to disable toasts
    },
  };

  return (
    <div>{isCallStarted && <JMConferenceView {...conferenceConfig} />}</div>
  );
}
```

## 🪝 Hooks

### 🤝 useJoin

This hook simplifies the process of joining meetings in your React application.

```typescript
useJoin {
  joinMeeting: (meetingData: IJMJoinMeetingParams) => Promise<void>;
  publishLocalPeerConfig: (config: IJMMediaSetting) => Promise<void>;
  isCallStarted: boolean;
}
```

##### Returns:

- **joinMeeting** :<code>joinMeeting(meetingData: <a href="#ijmjoinmeetingparams">IJMJoinMeetingParams</a>): Promise&lt;string&gt;</code>
  This function is used to join the call/Room
- **publishLocalPeerConfig** : <code>publish(config: <a href="#ijmmediasetting">IJMMediaSetting</a>): Promise&lt;string&gt;</code>
  After successfully joining the call , you can use this method to publish the initial audio , video, It allows you to publish audio and video tracks at a same time. This can be particularly useful when users select audio and video settings on a preview screen, or when you want to join a call with audio and video unmuted, or alternatively you can use audioUnmute and videoUnmute individually
- **isCallStarted** : optional `boolean`
  Weather you join the call or not , once you successfully join the call it give `True` otherwise `False`

##### Example:

```typescript
import { useJoin } from '@jiomeet/core-template-web';

function App() {
  const { joinMeeting, publishLocalPeerConfig, isCallStarted } = useJoin();
  const handleJoin = async () => {
    await joinMeeting({
      meetingId: '12345678',
      meetingPin: 'pqrst',
      userDisplayName: 'Rahul',
      config: { userRole: 'speaker' },
    });
  };

  return (
    <div>
      {isCallStarted ? 'Join Success' : 'Not Join Yet'}
      <div>
        <button onClick={() => handleJoin()}>Join</button>
      </div>
    </div>
  );
}
```

### ⚙️ useDevices

This Custom hook that provides access to audio and video devices and allows to change the the current audio/video device.

<p style="padding: 10px; background-color: rgba(255,229,100,.3); border-radius:5px"><span style="font-weight:bold">Important:</span> Note: Browsers give access to the list of devices only if the user has given permission to access them

```typescript
useDevices {
  getMediaPermissions: () => Promise<void>;
  allDevices: IJMDeviceMap;
  audioInputDevices: MediaDeviceInfo[];
  audioOutputDevices: MediaDeviceInfo[];
  videoInputDevices: MediaDeviceInfo[];
  setAudioInputDevice: (deviceId: string) => Promise<void>;
  setAudioOutputDevice: (deviceId: string) => Promise<void>;
  setVideoDevice: (deviceId: string) => Promise<void>;
  currentAudioInputDeviceId: string | undefined;
  currentAudioOutDeviceId: string | undefined;
  currentVideoInputDeviceId: string | undefined;
}
```

##### Returns:

- **getMediaPermissions** : <code>getMediaPermissions(): Promise&lt;void&gt;</code>
  This function is used to get microphone and camera permission
- **allDevices** : [`IJMDeviceMap`](#ijmdevicemap)
  It will return the object of type [`IJMDeviceMap`](#ijmdevicemap) which have array of audioInput,audioOutput,videoInput devices which consist webrtc [`MediaDeviceInfo`](https://developer.mozilla.org/en-US/docs/Web/API/MediaDeviceInfo)
- **audioInputDevices** : [`MediaDeviceInfo[]`](https://developer.mozilla.org/en-US/docs/Web/API/MediaDeviceInfo)
  It will return the list of all audioInput/Microphone devices available as a array of object type [`MediaDeviceInfo`](https://developer.mozilla.org/en-US/docs/Web/API/MediaDeviceInfo)
- **audioOutputDevices** : [`MediaDeviceInfo[]`](https://developer.mozilla.org/en-US/docs/Web/API/MediaDeviceInfo)
  It will return the list of all audioOutput/Speaker devices available as a array of object type [`MediaDeviceInfo`](https://developer.mozilla.org/en-US/docs/Web/API/MediaDeviceInfo)
- **videoInputDevices** : [`MediaDeviceInfo[]`](https://developer.mozilla.org/en-US/docs/Web/API/MediaDeviceInfo)
  It will return the list of all videoInput/Camera devices available as a array of object type [`MediaDeviceInfo`](https://developer.mozilla.org/en-US/docs/Web/API/MediaDeviceInfo)
- **setAudioInputDevice** : <code>setAudioInputDevice(deviceId:string): Promise&lt;void&gt;</code>
  It is a function which use to set audioInput/Microphone device in the room it take `deviceId` as a parameter
- **setAudioOutputDevice** : <code>setAudioOutputDevice(deviceId:string): Promise&lt;void&gt;</code>
  It is a function which use to set audioOutput/Speaker device in the room it take `deviceId` as a parameter
- **setVideoDevice** : <code>setVideoDevice(deviceId:string): Promise&lt;void&gt;</code>
  It is a function which use to set videoInput/Camera device in the room it take `deviceId` as a parameter
- **currentAudioInputDeviceId** : `string` | `undefined`
  It will return the current audioInput/Microphone devices used
- **currentAudioOutDeviceId** : `string` | `undefined`
  It will return the current audioOutput/Speaker devices used
- **currentVideoInputDeviceId** : `string` | `undefined`
  It will return the current videoInput/Camera devices used

##### Example:

```typescript
import { useDevices } from '@jiomeet/core-template-web';

function App() {
  const { currentAudioInputDeviceId, setAudioInputDevice } = useDevices();
  const optionChange = async (deviceId: string) => {
    await setAudioInputDevice(deviceId);
  };

  return (
    <div>
      {audioInputDevices.map(device => (
        <div key={device.deviceId} onClick={() => optionChange(device)}>
          {device.label}
        </div>
      ))}
    </div>
  );
}
```

### 👥 useRemotePeers

This hook give details of remote peers available in the call

```typescript
useRemotePeers {
  remotePeers: IJMRemotePeer[];
  getRemotePeerById: (id: string ) => IJMRemotePeer | null | undefined;
  isPeerDominantSpeaker: (id: string) => boolean;
}
```

##### Returns:

- **remotePeers** :[`IJMRemotePeer[]`](#ijmremotepeer)
  This will return array of object [`IJMRemotePeer`](#ijmremotepeer) of all the peer available in the room
- **getRemotePeerById** : <code>publish(id: string): Promise&lt;IJMRemotePeer | null | undefined;&gt;</code>
  This function give the specific remote peer info by taking peerId as a parameter
- **isPeerDominantSpeaker** : : <code>isPeerDominantSpeaker(id: string): boolean</code>
  Weather this particular user is currently top speaker or not

##### Example:

```typescript
import { useRemotePeers } from '@jiomeet/core-template-web';

function App() {
  const { remotePeers, getRemotePeerById, isPeerDominantSpeaker } =
    useRemotePeers();

  return (
    <div>
      {remotePeers.map(remotePeer => {
        return (
            <div>{`${remotePeer.name}`} </div>
            <div>mic: {remotePeer.audioMuted ? 'muted' : 'unmuted'}</div>
            <div> video: {remotePeer.videoMuted ? 'muted' : 'unmuted'}</div>
        )
      })}
    </div>
  );
}
```

### 👤 useLocalPeer

This hook give details of local peer and

```typescript
useLocalPeer {
  localPeer: IJMLocalPeer;
  playVideoTrack(trackId: string, element: string | HTMLElement): Promise<void>;
  leaveMeeting: () => Promise<void>;
}
```

##### Returns:

- **localPeer** :[`IJMLocalPeer`](#ijmlocalpeer)
  This will return of object [`IJMLocalPeer`](#ijmlocalpeer) which have all the details of local peer
- **playVideoTrack** : <code>playVideoTrack(trackId: string,element: string | HTMLElement): Promise&lt;void&gt;</code>
  This function use to play local video track , If you are using template then it will take care internally
- **leaveMeeting** : <code>leaveMeeting(): Promise&lt;void&gt;</code>
  This function is use to leave the meeting/Room

##### Example:

```typescript
import { useLocalPeer } from '@jiomeet/core-template-web';

function App() {
  const { leaveMeeting } = useLocalPeer();

  const handleLeave = async () => {
    await leaveMeeting();
  };

  return (
    <div>
      <button onClick={() => handleLeave()}>Join</button>
    </div>
  );
}
```

### 📽 useAVToggle

This hook give functionality to mute and unmute audio , video of local peer

```typescript
useAVToggle {
  isLocalAudioMuted: boolean;
  isLocalVideoMuted: boolean;
  toggleLocalAudio: () => Promise<void>;
  toggleLocalVideo: () => Promise<void>;
}
```

##### Returns:

- **isLocalAudioMuted** :`Boolean`
  This will return the current audio mute state of local peer , If Audio mute it return `True` other wise false
- **isLocalVideoMuted** :`Boolean`
  This will return the current video mute state of local peer , If Video mute it return `True` other wise false
- **toggleLocalAudio** : <code>toggleLocalAudio(): Promise&lt;void&gt;</code>
  This function use to toggle audio of local peer
- **toggleLocalVideo** : <code>toggleLocalVideo(): Promise&lt;void&gt;</code>
  This function use to toggle video of local peer

##### Example:

```typescript
import { useAVToggle } from '@jiomeet/core-template-web';

function App() {
  const { isLocalAudioMuted, toggleLocalAudio } = useAVToggle();

  const toggleMic = async () => {
    await toggleLocalAudio();
  };

  return (
    <div>
      <h1>{isLocalAudioMuted ? 'Your audio muted' : 'Your audio unmuted'} </h1>
      <button onClick={() => toggleMic()}>Join</button>
    </div>
  );
}
```

### 📽 useScreenShare

This hook give functionality to mute and mute audio , video of local peer

```typescript
useScreenShare {
  isLocalScreenShared: boolean;
  toggleScreenShare: () => Promise<void>;
  screenSharePeerId?: string | undefined;
}
```

##### Returns:

- **isLocalScreenShared** :`Boolean`
  This will return the current screen state of local peer , If Screen is shared by local peer it return `True` other wise `False`
- **toggleScreenShare** : <code>toggleScreenShare(): Promise&lt;void&gt;</code>
  This function use to toggle screen share of local peer
- **screenSharePeerId** : `String` | `undefined`
  This will return id of the peer who is currently sharing screen, will only be present if there is a screenshare in the room

##### Example:

```typescript
import { useScreenShare } from '@jiomeet/core-template-web';

function App() {
  const { toggleScreenShare, isLocalScreenShared } = useScreenShare();

  const toggleMyScreenShare = async () => {
    await toggleScreenShare();
  };

  return (
    <div>
      <h1>
        {isLocalScreenShared
          ? 'Your are sharing screen'
          : 'Your are not sharing screen'}
      </h1>
      <button onClick={() => toggleMyScreenShare()}>Join</button>
    </div>
  );
}
```

### 👤 useHostControls

This hook gives host control methods

```typescript
  useHostControls {
  toggleForceMute?: () => Promise<void>;
  softMute?:()=>Promise<void>;
  endMeeting?: () => Promise<void>;
  removePeerFromRoom?: (peerId: string) => Promise<void>;
  lowerRemotePeerHand: (peerId: string) => Promise<void>;
  toggleMeetingLock?: () => Promise<void>;
  isMeetingLocked: boolean;
  isHost: boolean;
  isRoomAudioForceMute: boolean;
}
```

##### Returns:

- **toggleForceMute** : <code>toggleForceMute(): Promise&lt;void&gt;</code>
  This function is use to toggle the force mute on the room. Participants will not be able to turn on their mic if room is on force mute.
- **softMute** : <code>softMute(): Promise&lt;void&gt;</code>
  This function is use to mute all the participants of the meeting
- **endMeeting** : <code>endMeeting(): Promise&lt;void&gt;</code>
  This function is use to end the meeting/Room for all
- **removePeerFromRoom** : <code>removePeerFromRoom(peerId:string): Promise&lt;void&gt;</code>
  This function is use to remove a specific peer from room/meeting
- **lowerRemotePeerHand** : <code>lowerRemotePeerHand(peerId:string): Promise&lt;void&gt;</code>
  This function is use to lower the hand of remote peer
- **toggleMeetingLock** : <code>toggleMeetingLock(): Promise&lt;void&gt;</code>
  This function is use to toggle the lock on the meeting
- **lowerRemotePeerHand** : <code>lowerRemotePeerHand(peerId:string): Promise&lt;void&gt;</code>
  This function is use to lower the hand of remote peer
- **isMeetingLocked** : optional `boolean`
  Weather the meeting is locked or unlocked ,if the meeting is locked it give `True` otherwise `False`
- **isHost** : `boolean`
  Weather the local user is host or not, if local user is host then returns `True` otherwise `False`

##### Example:

```typescript
import { useHostControls, useRemotePeers } from '@jiomeet/core-template-web';

function App() {
  const { endMeeting, removePeerFromRoom, isHost } = useHostControls();
  const { remotePeers } = useRemotePeers();

  const handleEndMeeting = async () => {
    if (!endMeeting) return;
    await endMeeting();
  };

  const handleRemovePeer = async (id: string) => {
    if (!removePeerFromRoom) return;
    await removePeerFromRoom(id);
  };

  return (
    <div>
      <button onClick={() => handleEndMeeting()}>End Meeting</button>
      {remotePeers.map(remotePeer => (
        <div key={remotePeer.peerId}>
          <div>{`${remotePeer.name}`}</div>
          <div>mic: {remotePeer.audioMuted ? 'muted' : 'unmuted'}</div>
          <div>video: {remotePeer.videoMuted ? 'muted' : 'unmuted'}</div>
          {isHost && (
            <button onClick={() => handleRemovePeer(remotePeer.peerId)}>
              Remove Peer
            </button>
          )}
        </div>
      ))}
    </div>
  );
}
```

### 👤 useJMEvents

This hook allows you to listen to different events from meeting

##### Example:

```typescript
import { useJMEvents } from '@jiomeet/core-template-web';
const peerDisconnectedEvent = useJMEvents('PEER_DISCONNECT_BY_HOST');
useEffect(() => {
  if (peerDisconnectedEvent.data.remotePeer) {
    showToast(
      `you were removed by,
      ${peerDisconnectedEvent.data.remotePeer.name}`,
    );
  }
}, [peerDisconnectedEvent]);
```

## 📖 Interfaces

#### `IJMJoinMeetingParams`

```typescript
interface IJMJoinMeetingParams {
  meetingId: string;
  meetingPin: string;
  userDisplayName: string;
  config: {
    userRole: 'host' | 'audience' | 'speaker';
    token?: string;
  };
}
```

#### `IJMRemotePeer`

```typescript
interface IJMRemotePeer {
  peerId: string;
  name: string;
  joinedAt?: string;
  metadata?: string;
  audioMuted: boolean;
  videoMuted: boolean;
  screenShare: boolean;
  isHost: boolean;
  audioTrack?: IJMRemoteAudioTrack;
  videoTrack?: IJMRemoteVideoTrack;
  screenShareTrack?: IJMRemoteScreenShareTrack;
}
```

#### `IJMLocalPeer`

```typescript
interface IJMLocalPeer {
  peerId: string;
  name: string;
  joinedAt?: string;
  metadata?: string;
  audioMuted: boolean;
  videoMuted: boolean;
  screenShare: boolean;
  isHost: boolean;
  audioTrack?: IJMLocalAudioTrack;
  videoTrack?: IJMLocalVideoTrack;
  screenShareTrack?: IJMLocalScreenShareTrack;
}
```

```typescript
interface IJMMediaSetting {
  trackSettings: {
    audioMuted: boolean;
    videoMuted: boolean;
    audioInputDeviceId?: string;
    audioOutputDeviceId?: string;
    videoDeviceId?: string;
  };
  virtualBackgroundSettings: {
    isVirtualBackground: boolean;
    sourceType: 'blur' | 'image' | 'none';
    sourceValue: string;
  };
}
```

#### `IJMConferenceConfigurations`

```typescript
export interface IJMConferenceConfigurations {
  configurations: {
    hideCallControls?: boolean;
    disableAudioOnlyMode?: boolean;
    defaultMediaSettings?: IJMMediaSetting;
    disableToasts?: boolean;
  };
}
```

#### `IJMDeviceMap`

```typescript
interface IJMDeviceMap {
  audioInput: MediaDeviceInfo[];
  audioOutput: MediaDeviceInfo[];
  videoInput: MediaDeviceInfo[];
}
```
