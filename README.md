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

1. **Create an `.npmrc` File**: Before installing the sdk, create an `.npmrc` file in the root directory of your project if it doesn't already exist.

2. **Configure Authentication**: In the `.npmrc` file,paste the authentication token shared by our team.

You can install the MeetingSDK using npm or yarn:

```bash
npm install @jiomeet/jm-web-core-template
```

## How to Integrate

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
		renderMeeting(this.meetingContainerRef.nativeElement);
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
		renderMeeting(meetingComponentRef.current);
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
