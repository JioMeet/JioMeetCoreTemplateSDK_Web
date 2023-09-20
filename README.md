# 🚀 JM Web Core Template Documentation

The JM Web Core template is a powerful sdk for integrating meetings into your Angular and React applications seamlessly. This SDK provides a simple method, `renderMeeting`, to render a meeting UI inside a specified HTML `div` element.

## Installation

You can install the MeetingSDK using npm or yarn:

```bash
npm install @jiomeet/jm-web-core-template
```

## Usage

### Creating Meeting

For creating meetings visit [dev.jiomeet.com](https://dev.jiomeet.com) platform after creating meeting pass that detail in the preview screen UI

which you will see after calling renderMeeting function as describe below.

### How to Integrate

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
