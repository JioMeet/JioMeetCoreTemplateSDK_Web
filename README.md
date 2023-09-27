# 🚀 JM Web Core Template Documentation

The JM Web Core template is a powerful sdk for integrating meetings into your Angular and React applications seamlessly. This SDK provides a simple method, `renderMeeting`, to render a meeting UI inside a specified HTML `div` element.

## Setup

#### Register on JioMeet Platform:

You need to first register on Jiomeet platform. [Click here to sign up](https://platform.jiomeet.com/login/signUp)

#### Get your application keys:

Create a new app. Please follow the steps provided in the [Documentation guide](https://dev.jiomeet.com/docs/quick-start/introduction) to create apps before you proceed.

#### Get your Jiomeet meeting id and pin

Use the [create meeting api](https://dev.jiomeet.com/docs/JioMeet%20Platform%20Server%20APIs/create-a-dynamic-meeting) to get your room id and password

## Installation

You can install the SDK using npm :

```bash
npm install @jiomeet/jm-web-core-template
```

## `renderMeeting`

The `renderMeeting` function is the core of the Core-template and is used to embed a meeting UI within your web application. It is the only function exposed from the sdk. It takes two important arguments:

##### Parameters:

- **element** : HTML div element
  The first argument is an HTML element where the meeting UI will be rendered.
- **meetingConfigurations** :[`IJMMeetingConfiguration`](#ijmmeetingconfiguration) | `Optional`
  - **meetingDetails**:[`IJMMeetingDetails`](#ijmmeetingdetails)
    Pass meeting details to auto-fill on the preview screen, or omit it to enter details manually.
    - **meetingId** : `string`  
      Meeting ID of the meeting user is going to
    - **meetingPin** : `string`  
       Meeting PIN of the meeting user is going to
    - **userDisplayName** : `string`  
       Display Name with which user is going to join the meeting.
    - **config** :
      - **userRole** : `"host" | "audience" | "speaker"`  
        Role of the user in the meeting. Possible values are .host, .speaker, .audience. If you are assigning .host value, please pass the token in its argument
      - **token?** `Optional` : `string`  
        Token is optional property only required when you pass userRole as a host

#### Angular

In your Angular component file:

```typescript
import { Component, ElementRef, ViewChild, AfterViewInit } from '@angular/core';
import { renderMeeting } from '@jiomeet/jm-web-core-template';

@Component({
  selector: 'app-meeting',
  template: '<div #meetingElement></div>',
})
export class MeetingComponent implements AfterViewInit {
  @ViewChild('meetingElement', { static: true })
  meetingContainerRef: ElementRef;

  ngAfterViewInit() {
    // Pass meeting details to auto-fill on the preview screen, or omit it to enter details manually.
    const meetingConfigurations = {
      meetingDetails{
      meetingId: 'yourMeetingId',
      meetingPin: 'yourMeetingPin',
      userDisplayName: 'Your Name',
      config: {
        userRole: 'host', // 'host', 'audience', or 'speaker'
        token: 'yourToken',
      },
      }
    };
    renderMeeting(this.meetingContainerRef.nativeElement,meetingConfigurations);
  }
}
```

In your Angular component's HTML template, include the app-meeting selector where you want to render the meeting:

```html
<app-meeting></app-meeting>
```

### React

In your React component file:

```typescript
import React, { useEffect, useRef } from 'react';
import { renderMeeting } from '@jiomeet/jm-web-core-template';

function MeetingComponent() {
  const meetingComponentRef = useRef(null);

  useEffect(() => {
    // Pass meeting details to auto-fill on the preview screen, or omit it to enter details manually.
    const meetingConfigurations = {
      meetingDetails{
      meetingId: 'yourMeetingId',
      meetingPin: 'yourMeetingPin',
      userDisplayName: 'Your Name',
      config: {
        userRole: 'host', // 'host', 'audience', or 'speaker'
        token: 'yourToken',
      },
      }
    };
    renderMeeting(meetingComponentRef.current,meetingConfigurations);
  }, []);

  return <div ref={meetingComponentRef}></div>;
}

export default MeetingComponent;
```

Render the MeetingComponent inside your main app component or wherever you need the meeting:

```typescript
import React from 'react';
import MeetingComponent from './MeetingComponent';

function App() {
	return (
		<div className="App">
			<MeetingComponent />
		</div>
	);
}

export default App;
```

**Interfaces**

#### `IJMMeetingConfiguration`

```typescript
export interface IJMMeetingConfiguration {
	meetingDetails?: IJMMeetingDetails;
}
```

#### `IJMMeetingDetails`

```typescript
export interface IJMMeetingDetails {
	meetingId: string;
	meetingPin: string;
	userDisplayName?: string;
	config?: {
		userRole?: 'host' | 'audience' | 'speaker';
		token?: string;
	};
}
```
